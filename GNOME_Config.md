## Manjaro GNOME Config
The base installation is from https://manjaro.org/download/#gnome.
## Update before anything 
`sudo pacman -Syu`

### Install some dependencies 
`sudo pacman -S gnome-tweaks ttf-opensans ttf-hack`

### GTK Config 
Nordic Polar Standart Buttons or Nordic Standart Buttons https://www.gnome-look.org/p/1267246/

Copy to ~/.themes

Icons: Papirus Dark

### Extensions
- Activities Configurator: https://extensions.gnome.org/extension/358/activities-configurator/
- Arc Menu: https://extensions.gnome.org/extension/1228/arc-menu/
- Dynamic Panel Transparency: https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/
- Impatience: https://extensions.gnome.org/extension/277/impatience/
- KStatusNotifierItem/AppIndicator Support: https://extensions.gnome.org/extension/615/appindicator-support/
- Open Weather: https://extensions.gnome.org/extension/750/openweather/
- Removable Drive Menu: https://extensions.gnome.org/extension/7/removable-drive-menu/
- User Themes: https://extensions.gnome.org/extension/19/user-themes/
- Dash to Panel: https://extensions.gnome.org/extension/1160/dash-to-panel/
- Pamac Updates Indicator

### Fonts ##
- Interface Text: Open Sans 12
- Document Text: Open Sans 12
- Monospace Text: Hack Regular 12
- Legacy Windows Titles: Open Sans Bold 12
- Hiting: Slight; Antialiasing Subpixel; Scale: 1

## i3 Config 
`sudo pacman -S i3-gaps dmenu rofi i3blocks i3status i3lock nemo lxappearance sysstat kitty dunst picom playerctl udiskie feh nethogs volumeicon clipit unclutter ttf-opensans ttf-hack arandr neofetch zensu`

Optional: install oh my zsh https://github.com/ohmyzsh/ohmyzsh

### Default terminal
Add/change /etc/environment var TERMINAL=kitty

`sudo echo "TERMINAL=kitty" >> /etc/environment`
`sudo nano /etc/environment`

### i3 config files ##
Clone/Download files from this repo to .config folder and .Xresources to home
- ~/.config/i3/i3exit: Change permission to execute
- ~/.config/i3blocks/custom_scripts: Change permissions to execute

### GTK Config ## 
Nordic Polar Standart Buttons or Nordic Standart Buttons https://www.gnome-look.org/p/1267246/
Copy to ~/.themes
Open lxappearance and change theme and fonts to opens sans - 12, hack - 12
Icons: Papirus Dark

### Lutris ###
#### Information from lutris github ##
- https://github.com/lutris/docs/blob/master/HowToDXVK.md
- https://github.com/lutris/docs/blob/master/WineDependencies.md
- https://github.com/lutris/docs/blob/master/InstallingDrivers.md

### Install ##
`sudo pacman -S lutris`

### Enable multilib 
`sudo nano /etc/pacman.conf`

`[multilib]`

`Include = /etc/pacman.d/mirrorlist`

### Update ##
`sudo pacman -Syu`

### Drivers and DXVK Config ##
#### Nvidia #
`sudo pacman -S nvidia-dkms nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader`
#### AMD #
`sudo pacman -S lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader`
#### Intel #
`sudo pacman -S lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader`

### Wine Config ##
`sudo pacman -S wine-staging giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader`

#### Addtional dependencies for battle.net ##
`sudo pacman -S lib32-gnutls lib32-libldap lib32-libgpg-error lib32-sqlite lib32-libpulse`

### MangoHUD
https://github.com/flightlessmango/MangoHud
### Install
`pamac install mangohud`

#### Test DXVK and MandoHUD
`mangohud glxgears`
