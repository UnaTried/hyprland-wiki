---
weight: 10
title: File Managers
---

## GUI

- [dolphin](https://github.com/KDE/dolphin): File manager by KDE.
- [nautilus](https://apps.gnome.org/en/Nautilus/): File manager by Gnome.
  - [nautilus-admin-gtk4](https://github.com/MacTavishAO/nautilus-admin-gtk4): Open files with elevated privileges.
      - [nautilus-image-converter](https://gitlab.gnome.org/coreyberla/nautilus-image-converter): Resize and rotate images.
        - [nautilus-open-any-terminal](https://github.com/Stunkymonkey/nautilus-open-any-terminal): Open terminals in selected directory.
        - [nemo](https://github.com/linuxmint/nemo): File manager by Cinnamon.
          - [nemo-fileroller](https://github.com/linuxmint/nemo-extensions/tree/master/nemo-fileroller): File archiver extension.
            - [nemo-terminal](https://github.com/linuxmint/nemo-extensions/tree/master/nemo-terminal): Embedded terminal window.
            - [pcmanfm-qt](https://github.com/lxqt/pcmanfm-qt): File manager by LXQt.
            - [pcmanfm](https://github.com/lxde/pcmanfm): File manager by LXDE.
            - [thunar](https://gitlab.xfce.org/xfce/thunar): File manager by XFCE.

            ## TUI

            - [lf](https://github.com/gokcehan/lf): A filemanager heavily inspired by ranger written in Go.
            - [nnn](https://github.com/jarun/nnn): n³ The Unorthodox terminal file manager.
            - [ranger](https://github.com/ranger/ranger): A VIM-inspired filemanager for the console.
            - [superfile](https://github.com/yorukot/superfile): Pretty fancy and modern terminal file manager.
            - [yazi](https://yazi-rs.github.io/): Blazing fast terminal file manager written in Rust, based on async I/O.
            - [xdg-desktop-portal-termfilechooser](https://github.com/hunkyburrito/xdg-desktop-portal-termfilechooser): Forked by hunkyborrito; A desktop portal that allows users to use their TUI file managers as a file picker/chooser.

            ---

            # XDPtfc

            Some linux distros require moving the .portal file for this to work are Alpine/postmarketOS, Debian, and Arch Linux: `sudo mv /usr/local/share/xdg-desktop-portal/portals/termfilechooser.portal /usr/share/xdg-desktop-portal/portals/`

            ## Usage

            Put `exec-once = /usr/local/lib/xdg-desktop-portal-termfilechooser -r` into your hyprland config. If your xdg-desktop-portal version is 1.18.0 or above create `~/.config/xdg-desktop-portal/portals.conf
            and add:
            ```
            [preferred]
            org.freedesktop.impl.portal.FileChooser=termfilechooser
            ```
            if it's under that version, then you can remove the individual file picker files, so xdg-desktop-portal automatically chooses XDGtfc. To do that run:

            ```
            find /usr/share/xdg-desktop-portal/portals -name '*.portal' -not -name 'termfilechooser.portal' \
            	-exec grep -q 'FileChooser' '{}' \; \
                -exec sudo sed -i'.bak' 's/org\.freedesktop\.impl\.portal\.FileChooser;\?//g' '{}' \;
                ```

                Configuration is done in `~/.config/xdg-desktop-portal-termfilechooser/config` using the INI format. Copy `/use/local/share/xdg-desktop-portal-termfilechooser/` into `~/. config/xdg-desktop-portal-termfilechooser/`

                ### Configuration
                Current only the `[filechooser]` section exists

                | variable | description | type | default
                | --- | --- | --- | --- |
                | cmd | the wrapper script for your file manager, relative to the config file, /usr/(local/)share/xdg-desktop-portal-termfilechooser, /usr/share/xdg-desktop-portal-termfilechooser or everything in $PATH | string | yazi-wrapper.sh |
                | create_help_file | if the file that shows you the instructions should be created, either 1 or 0 | bool | 1 |
                | default_dir | the default directory that gets used when the picking application doesn't input a path to open | string | ~ else /tmp |
                | env | sets any environment while in the file picker | environment variable | empty |
                | env=TERMCMD | what command to use to open the file picker | string | empty |
                | open_mode | what directory to open the file picker at, e.g. the default one (default_dir), suggested (from the picking application), the directory of the last selected file | string | suggested |
                | save_mode | the same as open mode, but for saving files not picking | string | suggested |

                #### Examples
                ```
                [filechooser]
                cmd=yazi-wrapper.sh
                default_dir=$HOME
                env=TERMCMD=foot -T "terminal filechooser"
                    VARIABLE2=VALUE2
                    open_mode = suggested
                    save_mode = last
                    ```

                    ```
                    [filechooser]
                    cmd=TERMCMD='foot -T "terminal filechooser"' yazi-wrapper.sh
                    default_dir=$HOME
                    env=VARIABLE2=VALUE2
                    open_mode = suggested
                    save_mode = last
                    ```

                    ## Tips
                    ### Firefox
                    Firefox has a setting to always use XDG desktop portal's file chooser.

                    Open the site `about:config`, then set `widget.use-xdg-desktop-portal.file-picker` to `1`. See [the Arch Wiki](https://wiki.archlinux.org/title/Firefox#XDG_Desktop_Portal_integration) for more information.

                    ### Setting TERMCMD somewhere else
                    Inside the hyprland config:
                    `env = TERMCMD,foot -T "terminal filechooser"`
                    In Your shell config:
                    `export TERMCMD='foot -T "terminal filechooser"'`

                    ### Modifying an existing wrapper or making your own 
                    Since we already copied all the file manager wrappers earlier it's