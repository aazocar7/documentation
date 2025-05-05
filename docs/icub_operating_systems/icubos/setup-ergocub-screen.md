# Setup ergoCub Screen

!!! info
    This procedure applies only to `ergocub-head` and must be performed after the [JetPack installation](./jetpack.md) has been completed.

When you start the ergoCub head for the first time, the screen will display a desktop showing the NVIDIA logo. While this is generally fine, it requires the user to manually run the [`ergoCubEmotions`](https://github.com/icub-tech-iit/ergocub-software/tree/master/src/modules/ergoCubEmotions) module. This procedure allows you to hide all bars on the desktop and set the wallpaper to one of the images stored in the [expressions folder](https://github.com/icub-tech-iit/ergocub-software/tree/14599254440686b8a373e1635f046a6821ddee78/src/modules/ergoCubEmotions/expressions/images).

---

## Hide the Ubuntu Bar

This guide is based on these [instructions](https://askubuntu.com/a/1264692). 

1. Create a script in the `ergocub-head` home directory:

    ```bash
    #!/bin/bash

    status1=$(gdbus call --session --dest org.gnome.Shell --object-path /org/gnome/Shell --method org.gnome.Shell.Eval string:'Main.panel.actor.visible;')
    status2=$(gdbus call --session --dest org.gnome.Shell.Extensions --object-path /org/gnome/Shell/Extensions --method org.gnome.Shell.Extensions.GetExtensionInfo ubuntu-dock@ubuntu.com | grep "'state': <2.0>" >/dev/null && echo "OFF" || echo "ON")

    if [ "$status1" == "(true, 'false')" ]; then
      gdbus call --session --dest org.gnome.Shell --object-path /org/gnome/Shell --method org.gnome.Shell.Eval 'Main.panel.actor.show();'
    else
      gdbus call --session --dest org.gnome.Shell --object-path /org/gnome/Shell --method org.gnome.Shell.Eval 'Main.panel.actor.hide();'
    fi

    if [ "$status2" == "ON" ]; then
      gdbus call --session --dest org.gnome.Shell.Extensions --object-path /org/gnome/Shell/Extensions --method org.gnome.Shell.Extensions.DisableExtension ubuntu-dock@ubuntu.com
    else
      gdbus call --session --dest org.gnome.Shell.Extensions --object-path /org/gnome/Shell/Extensions --method org.gnome.Shell.Extensions.EnableExtension ubuntu-dock@ubuntu.com
    fi
    ```

2. Run the script to toggle hiding or showing the bars.

---

## Change the Desktop Wallpaper

1. SSH into the `ergocub-head` and use the following command to set the wallpaper:

    ```bash
    gsettings set org.gnome.desktop.background picture-uri <uri-of-the-wallpaper>
    ```

2. If [ergocub-software](https://github.com/icub-tech-iit/ergocub-software) is installed via the robotology-superbuild, you can use this command:

    ```bash
    gsettings set org.gnome.desktop.background picture-uri file:///usr/local/src/robot/robotology-superbuild/src/ergocub-software/src/modules/ergoCubEmotions/expressions/images/exp_img_1.png
    ```

---

## Clean the Icons from the Desktop

1. Create a folder in the home directory to store the desktop files:

    ```bash
    mkdir ~/all_desktop
    ```

2. Move all files from the desktop to the newly created folder:

    ```bash
    mv ~/Desktop/* ~/all_desktop/
    ```
