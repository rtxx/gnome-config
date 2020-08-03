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
  
  `Since there is no option, you can only remove them by changing the code. Delete line frameLoader.iconShadow = ... from the end of file FolderItemDelegate.qml; should be around line number 520. On my system, the file as at /usr/share/plasma/plasmoids/org.kde.desktopcontainment/contents/ui/FolderItemDelegate.qml`
    
