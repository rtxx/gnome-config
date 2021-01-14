# Minimal Arch Install
## General install steps
1. Connect to the internet

    Connecting to a network with ```iwcl``` or wired connecting
   
    Update system clock 
2. Create partitions and mount them

   Creating partitions using ```cfdisk```, format and mount
  
3. Install arch

   Installing arch using ```pacstrap```

   Config ```fstab timezone locale-gen keyboard-layout hostname hosts dhcpcd initramfs grub```

4. Config it

## Pre-Install and connect to the internet
Load pt keyboard
```
loadkeys pt-latin1
```

Wireless configuration
```iwctl```
* List devices
```device list```
* Scan networks
```station [device] scan```
* Get avaiable networks
```station [device] get-networks```
* Connect to a networks
```station [device] connect [SSID]```
* Check connection
```ping www.google.com```

Update system clock
```
timedatectl set-ntp true
```

## Disk partition
My default config with 3 partitions
Open ```cfdisk``` and create
* boot with at least 512MB, should be /dev/sdx1
* swap with atleast 1GB, should be /dev/sdx2
* root with the remaining, should be /dev/sdx3

Format the partitions 
```
mkfs.ext4 /dev/sdx1
mkswap /dev/sdx2
mkfs.ext4 /dev/sdx3
``` 
and mount them
```
mount /dev/sdx3 /mnt
mkdir /mnt/{boot,home}
mount /dev/sdx1 /mnt/boot
swapon /dev/sdx2 
```

## Install Arch
* Install base system and extras 
```
pacstrap /mnt base linux linux-firmware nano dhcpcd grub efibootmgr os-prober
```

* Install Microcode - https://wiki.archlinux.org/index.php/Microcode

>Processor manufacturers release stability and security updates to the processor microcode. These updates provide bug fixes that can be critical to the stability of your system. Without them, you may experience spurious crashes or unexpected system halts that can be difficult to track down.

Amd: amd-ucode or Intel: intel-ucode
```
pacstrap /mnt amd-ucode
```

* Generate fstab - https://wiki.archlinux.org/index.php/Fstab

>The fstab file can be used to define how disk partitions, various other block devices, or remote filesystems should be mounted into the filesystem.

```
genfstab -U /mnt >> /mnt/etc/fstab
```

* ```chroot``` into the new system - https://wiki.archlinux.org/index.php/Chroot

>A chroot is an operation that changes the apparent root directory for the current running process and their children.

```
arch-chroot /mnt
```

* Set the time zone - https://wiki.archlinux.org/index.php/System_time#Time_zone
```
timedatectl set-timezone Europe/Lisbon
```

* Generate /etc/adjtime - https://wiki.archlinux.org/index.php/System_time#Set_hardware_clock_from_system_clock - https://man.archlinux.org/man/hwclock.8#The_Adjtime_File

>The following sets the hardware clock from the system clock. Additionally it updates /etc/adjtime or creates it if not present.

```
hwclock --systohc
```

* Select locale in /etc/locale.gen (uncomment) and gen it

>Locales are used by glibc and other locale-aware programs or libraries for rendering text, correctly displaying regional monetary values, time and date formats, alphabetic idiosyncrasies, and other locale-specific standards.

```
nano locale-gen
```
```
locale-gen
```
* Create locale.conf and set the language - https://wiki.archlinux.org/index.php/Locale#Setting_the_system_locale


>To set the system locale, write the LANG variable to /etc/locale.conf, where en_US.UTF-8 belongs to the first column of an uncommented entry in /etc/locale.gen
```
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
```

* Set keyboard layout
```
echo "KEYMAP=pt-latin1" >> /etc/vconsole.conf
```

* Set hostname - https://wiki.archlinux.org/index.php/Network_configuration#Set_the_hostname

>A hostname is a unique name created to identify a machine on a network, configured in /etc/hostname

```
echo "archx64" >> /etc/hostname
```

* Config /etc/hosts - https://wiki.archlinux.org/index.php/Network_configuration#Local_hostname_resolution
```
127.0.0.1    localhost
::1          localhost
127.0.1.1    archx64.localdomain     archx64
```
```
echo -e "127.0.0.1	localhost\n::1		localhost\n127.0.1.1	archx64.localdomain	archx64" >> /etc/hosts
```

* Enable dhcpcd service - https://wiki.archlinux.org/index.php/Dhcpcd

>dhcpcd is a DHCP and DHCPv6 client
```
systemctl enable dhcpcd
```

* Recreate initramfs - https://wiki.archlinux.org/index.php/Arch_boot_process#initramfs
```
mkinitcpio -P
```

* Config bootloader - https://wiki.archlinux.org/index.php/Arch_boot_process#Boot_loader

>A boot loader is a piece of software started by the firmware (BIOS or UEFI). It is responsible for loading the kernel with the wanted kernel parameters, and initial RAM disk based on configuration files. In the case of UEFI, the kernel itself can be directly launched by the UEFI using the EFI boot stub. A separate boot loader or boot manager can still be used for the purpose of editing kernel parameters before booting.

GRUB - https://wiki.archlinux.org/index.php/GRUB
* MBR ```grub-install --target=i386-pc /dev/sdx```
* UEFI ```grub-install --target=x86_64-efi --efi-directory=/mnt/boot --bootloader-id=archlinux```

Generate main config file for grub
```
grub-mkconfig -o /boot/grub/grub.cfg
```

* Set root password
```
passwd
```

* Exit chroot and reboot!
```
exit && reboot
```

#### Arch should now be installed, login with root

## Config the new install
Install sudo and bash completion
```
pacman -S sudo bash-completion
```

Edit sudoers and remove the comment on %wheel
```
EDITOR=nano visudo
```
```
## Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL) ALL
```
Create user and add them to wheel group
```
useradd -m -G wheel [user]
```
https://wiki.archlinux.org/index.php/sudo#Example_entries

>When creating new administrators, it is often desirable to enable sudo access for the wheel group and add the user to it, since by default Polkit treats the members of the wheel group as administrators. If the user is not a member of wheel, software using Polkit may ask to authenticate using the root password instead of the user password.


Add a password to the new created user
```
passwd [user]
```

Exit root and login with new user
```
exit
```

Test permissions of the new user
```
sudo pacman -Syu
```

#### At this point, arch is installed and ready to use, although it is very *very* bare bones. Highly recommended installing atleast a firewall and a WM, like i3 or a DE like xfce.

## Extras

### Enabling multilib
```
sudo nano /etc/pacman.conf
``` 
and uncomment
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
### Installing X - https://wiki.archlinux.org/index.php/Xorg
```
sudo pacman -S xorg-server xorg-xinit xorg-setxkbmap
```
or install xorg group with a couple of extras
```
sudo pacman -S xorg xorg-xinit
```
Setting the keyboard layout - https://wiki.archlinux.org/index.php/Xorg/Keyboard_configuration#Setting_keyboard_layout
```
localectl set-x11-keymap pt
```

### Installing graphic drivers 

Check https://wiki.archlinux.org/index.php/Xorg#Driver_installation. Example: Intel
```
sudo pacman -S xf86-video-intel mesa lib32-mesa
```

### Window Manager - i3

https://wiki.archlinux.org/index.php/i3

Config ```.xinitrc```, from https://wiki.manjaro.org/index.php/Proper_~/.xinitrc_File
```
nano ~/.xinitrc
```
```
#!/bin/sh
#
# ~/.xinitrc
#
# Executed by startx (run your window manager from here)

if [ -d /etc/X11/xinit/xinitrc.d ]; then
  for f in /etc/X11/xinit/xinitrc.d/*; do
    [ -x "$f" ] && . "$f"
  done
  unset f
fi
exec i3
```
Install i3 and some extras
```
sudo pacman -S i3-wm i3status i3lock dmenu ttf-opensans noto-fonts ttf-fantasque-sans-mono xterm htop
```
Optional: i3 layout based on manjaro i3 - https://github.com/rtxx/linux-config/blob/master/.config/i3/config . Copy to ~/.config/i3.
Warning! If this layout is used, there's a bunch of programs not installed yet.

### Desktop Environment - xfce

https://wiki.archlinux.org/index.php/Xfce

Config ```.xinitrc```, from https://wiki.manjaro.org/index.php/Proper_~/.xinitrc_File
```
nano ~/.xinitrc
```
```
#!/bin/sh
#
# ~/.xinitrc
#
# Executed by startx (run your window manager from here)

if [ -d /etc/X11/xinit/xinitrc.d ]; then
  for f in /etc/X11/xinit/xinitrc.d/*; do
    [ -x "$f" ] && . "$f"
  done
  unset f
fi
exec startxfce4
```
Install xfce and some extras
```
sudo pacman -S xfce4 xfce4-goodies noto-fonts ttf-fantasque-sans-mono htop
```

Start x server
```
startx
```

#### Right now you should have a bare minimium graphic environment to use. Highly recommended install a DM, like lightdm.

### User Directories
Install ```xdg-user-dirs```
```
pacman -S xdg-user-dirs
```
and run
```
xdg-user-dirs-update
```
to create the default directories in /home (Documents, Downloads, etc)

### Firewall
Install cfw with a basic config - https://wiki.archlinux.org/index.php/Uncomplicated_Firewall
```
sudo pacman -S ufw
```
Basic config
```
ufw default deny && ufw allow from 192.168.0.0/24 && ufw limit ssh
```
Enable it and start it
```
ufw enable
systemctl enable ufw
systemctl start ufw
ufw status
```
Install gufw, GUI frontend for ufw
```
sudo pacman -S gufw
```
### Sound

Installing PulseAudio with ALSA plugin - https://wiki.archlinux.org/index.php/PulseAudio
```
sudo pacman -S pulseaudio pulseaudio-alsa alsa-utils pavucontrol
```
Check sound levels with ```alsamixer``` or ```pavucontrol``` install ```volumeicon``` for a tray icon
If ```paucontrol``` wont work, reboot and check again, if still the same, open as user (not root) and check it.

### Display Manager

Installing LightDM - https://wiki.archlinux.org/index.php/LightDM
```
sudo pacman -S lightdm lightdm-gtk-greeter
```
Enabling it
```
systemctl enable lightdm
```
Install ``` lightdm-gtk-greeter-settings``` to change appearance of it

### Keyring - Manage passwords

Installing GNOME Keyring to manage passwords from programs, wifi keys, ssh, etc. - https://wiki.archlinux.org/index.php/GNOME/Keyring
```
sudo pacman -S gnome-keyring
```
For GUI, install ```seahorse```
```
sudo pacman -S seahorse
```
Auto unlock keyring

Without a DM, using ```.xinitrc```
```
eval $(/usr/bin/gnome-keyring-daemon --start --components=pkcs11,secrets,ssh)
export SSH_AUTH_SOCK
```
Using a DM, like LightDM, it should be automatic. However if it is not unlocked on login, open ```seahorse```, create a new *keyring* with the name ```login``` and the password should be the *same* as the current user.

To enable the keyring for terminal applications update ```.bash_profile``` with
```
if [ -n "$DESKTOP_SESSION" ];then
    eval $(gnome-keyring-daemon --start)
    export SSH_AUTH_SOCK
fi
```
### Appearance

Install Breeze theme - https://wiki.archlinux.org/index.php/Uniform_look_for_Qt_and_GTK_applications
```
sudo pacman -S breeze breeze-gtk breeze-icons qt5ct lxappearance
```
* Config QT
   ```
   echo "QT_QPA_PLATFORMTHEME=qt5ct" >> /etc/environment
   ```
   Open ```qt5ct``` and change theme to "breeze" and icons.

* Config GTK

   Open ```lxappearance``` and change theme and icons.

Xresources - https://github.com/rtxx/linux-config/blob/master/.Xresources . Copy to ```~/.Xresources```


Alternative theme - Materia - https://github.com/nana-4/materia-theme
```
sudo pacman -S kvantum kvantum-theme-materia materia-gtk-theme capitaine-cursors arc-icon-theme lxappearance
```
* Config QT
   ```
   echo "QT_QPA_PLATFORMTHEME=qt5ct" >> /etc/environment
   ```
   Open ```qt5ct``` and change theme to "kvantum" and icons.

* Config GTK

   Open ```lxappearance``` and change theme and icons.
   
* Config Kvantum

   Open ```kvantummanager``` and change theme.
   
Xresources - Monokai colors - https://github.com/logico-dev/Xresources-themes/blob/master/base16-monokai.Xresources . Copy to ```~/.Xresources```

   
Wallpapers
```
sudo pacman -S archlinux-wallpaper
```
Wallpapers are located in ```/usr/share/backgrounds/archlinux/```

Fonts

Open ```lxappearance``` and ```qt5ct``` change to ```Noto Sans Regular``` ```Fantasque Sans Mono```

