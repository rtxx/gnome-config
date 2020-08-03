# KDE Plasma Configs

- Plasma Style - Breeze
- Colors - McMojaveLight - https://store.kde.org/p/1305007
- Fonts - Open Sans - https://fonts.google.com/specimen/Open+Sans
- Icons - Darkine - https://store.kde.org/p/1304954

### Widgets
##### Latte Dock Widgets - In Order
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
- Spacing (old version): Open `/usr/share/plasma/plasmoids/org.kde.plasma.private.systemtray/contents/ui/main.qml`. Search for `//Main Layout` and change spacing to 8. Save and relog. **Updates WILL break this.**
- Spacing: https://github.com/psifidotos/plasma-systray-Latte-Tweaks
- Turn off Media Player and Notifications
- Notifications - Default Widget
- Redshift Control - https://github.com/kotelnik/plasma-applet-redshift-control OR Night Color Control - Default Widget
- Digital Clock - Default Widget
- Weather Report - Default Widget
- Window Buttons - https://github.com/psifidotos/applet-window-buttons - Arch AUR - https://aur.archlinux.org/packages/plasma5-applets-window-title/
- Use where needed
- Latte Separator - https://github.com/psifidotos/applet-latte-separator/
- Latte Spacer - https://github.com/psifidotos/applet-latte-spacer

###  Disable desktop icon shadows
https://www.reddit.com/r/kde/comments/d6jfe3/any_way_to_disable_new_desktop_icon_shadows/

Open `/usr/share/plasma/plasmoids/org.kde.desktopcontainment/contents/ui/FolderItemDelegate.qml`, search for `frameLoader.textShadow `, delete or add //.

### Gnome Keyring
#### Installation
https://wiki.archlinux.org/index.php/GNOME/Keyring

`sudo pacman -S  gnome-keyring libsecret seahorse`

Open seahorse and change the default wallet to login.

#### Automatically unlock in SDDM+KDE
https://en.opensuse.org/GNOME_Keyring

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

    
