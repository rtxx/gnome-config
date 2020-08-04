# KDE Plasma Configs

- Plasma Style 
    - Breeze
    - GTK Theme - Breeze Blur Blue - https://store.kde.org/p/1289302
    - Colors - Breeze Light
- Fonts
    - Open Sans - https://fonts.google.com/specimen/Open+Sans
    - IBM Plex - https://www.ibm.com/plex/
    - Segoe UI (Optional) - Extract from Windows Install
- Icons 
    - Darkine - https://store.kde.org/p/1304954
    - We10X - https://github.com/yeyushengfan258/We10X-icon-theme

## Widgets & Layout
### Latte Dock Widgets - In Order - MacOS Inspired
- Application Launcher - Default Widget
- Window Title - https://github.com/psifidotos/applet-window-title
- Global Menu - https://github.com/psifidotos/applet-window-appmenu - Arch AUR - https://aur.archlinux.org/packages/plasma5-applets-window-appmenu/
- Command Output - https://github.com/Zren/plasma-applet-commandoutput 
- playerctl - https://github.com/altdesktop/playerctl
- Command - `playerctl --player=elisa,spotify,de.haeckerfelix.Shortwave,vlc,chromium metadata --format " {{ artist }} - {{ title }}"`
- On Click - `playerctl --player=elisa,spotify,de.haeckerfelix.Shortwave,vlc,chromium play-pause`
- Mouse Wheel Up - `playerctl --player=elisa,spotify,de.haeckerfelix.Shortwave,vlc,chromium next`
- Mouse Wheel Down - `playerctl --player=elisa,spotify,de.haeckerfelix.Shortwave,vlc,chromium previous`
- Media Player - Defaut Widget
- System Tray - Default Widget
- Spacing: https://github.com/psifidotos/plasma-systray-Latte-Tweaks
    - (old version): Open `/usr/share/plasma/plasmoids/org.kde.plasma.private.systemtray/contents/ui/main.qml`. Search for `//Main Layout` and change spacing to 8. Save and relog. **Updates WILL break this.**
- Turn off Media Player and Notifications
- Notifications - Default Widget
- Redshift Control - https://github.com/kotelnik/plasma-applet-redshift-control OR Night Color Control - Default Widget
- Digital Clock - Default Widget
- Weather Report - Default Widget
- Window Buttons - https://github.com/psifidotos/applet-window-buttons - Arch AUR - https://aur.archlinux.org/packages/plasma5-applets-window-title/
- Use where needed
- Latte Separator - https://github.com/psifidotos/applet-latte-separator/
- Latte Spacer - https://github.com/psifidotos/applet-latte-spacer
### Windows Inspired
 - Tiled Menu - https://store.kde.org/p/1160672
 - Search - Default Widget
 - Icons Only Task Manager - Default Widget
 - System Tray - Default Widget with spacing from https://github.com/psifidotos/plasma-systray-Latte-Tweaks
 - Clock - Default Widget
 - Show Desktop - https://store.kde.org/p/1100895
 
###  Disable desktop icon shadows
https://www.reddit.com/r/kde/comments/d6jfe3/any_way_to_disable_new_desktop_icon_shadows/

Open `/usr/share/plasma/plasmoids/org.kde.desktopcontainment/contents/ui/FolderItemDelegate.qml`, search for `frameLoader.textShadow `, delete or add //.

## Gnome Keyring
### Installation
https://wiki.archlinux.org/index.php/GNOME/Keyring

`sudo pacman -S  gnome-keyring libsecret seahorse`

Open seahorse and change the default wallet to login.

### Automatically unlock in SDDM+KDE
https://en.opensuse.org/GNOME_Keyring

https://forum.manjaro.org/t/automatically-unlock-gnome-keyring-on-login-kde-sddm/51016

Edit `/etc/pam.d/sddm ` and the two lines:

```
#%PAM-1.0
auth     include        common-auth
auth     optional       pam_gnome_keyring.so <--
account  include        common-account
password include        common-password
session  required       pam_loginuid.so
session  include        common-session
session  optional       pam_gnome_keyring.so auto_start <--
```
The two lines must be inserted in the exact position. Otherwise it might not work.
The login password **must** be the same for this to work.

    
