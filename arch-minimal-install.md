# Minimal Arch install with i3
## General install steps
1. Connect to the internet
2. Create partitions and mount them
3. Install arch 
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
Install base system and extras 
```
pacstrap /mnt base linux linux-firmware nano dhcpcd grub efibootmgr
```

Install microde, choose amd: amd-ucode or intel: intel-ucode
```
pacstrap /mnt amd-ucode
```

Generate fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```

```chroot``` into the new system
```
arch-chroot /mnt
```

Set the time zone
```
timedatectl set-timezone Europe/Lisbon
```

Generate /etc/adjtime
```
hwclock --systohc
```

Select locale in /etc/locale.gen (uncomment) and gen it
```
nano locale-gen
locale-gen
```
Create locale.conf and set the language
```
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
```

Set keyboard layout
```
echo "KEYMAP=pt-latin1" >> /etc/vconsole.conf
```

Set hostname
```
echo "archx64" >> /etc/hostname
```

Config /etc/hosts
```
echo -e "127.0.0.1	localhost\n::1		localhost\n127.0.1.1	archx64.localdomain	archx64" >> /etc/hosts
```

Update the system
```
pacman -Syu
```

Enable dhcpcd service
```
systemctl enable dhcpcd
```

Recreate initramfs
```
mkinitcpio -P
```

Config bootloader (grub)
* MBR ```grub-install --target=i386-pc /dev/sdx```
* UEFI ```grub-install --target=x86_64-efi --efi-directory=/mnt/boot --bootloader-id=archlinux```

Generate main config file for grub
```
grub-mkconfig -o /boot/grub/grub.cfg
```

Set root password
```
passwd
```

Exit chroot and reboot!
```
exit && reboot
```

#### Arch should now be installed, login with root

## Configuration of the new install
Install sudo
```
pacman -S sudo
```

Edit sudoers and remove the comment on %wheel
```
EDITOR=nano visudo
```

Create user and add them to wheel group
```
useradd -m -G wheel [user]
```

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

## Extras

Enabling multilib
```
sudo nano /etc/pacman.conf
``` 
and uncomment
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
X install
```
sudo pacman -S xorg-server
```
or install xorg group with a couple of extras
```
sudo pacman -S xorg xorg-xinit
```

Installing drivers, check https://wiki.archlinux.org/index.php/Xorg#Driver_installation 

Example: Intel
```
sudo pacman -S xf86-video-intel mesa lib32-mesa
```

Desktop Env - i3 as an example

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
sudo pacman -S i3-wm i3status i3lock dmenu ttf-hack xterm htop
```

Start x server
```
startx
```

#### Right now you should have a bare minimium graphic env to use

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
ufw default deny && ufw allow from 192.168.0.0/24 && ufw allow Deluge && ufw limit ssh
```
Enable it and start it
```
ufw enable
systemctl enable ufw
systemctl start ufw
ufw status
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
Install ``` lightdm-gtk-greeter-settings``` to change appereance of it

### Appearance
Install Breeze theme - https://wiki.archlinux.org/index.php/Uniform_look_for_Qt_and_GTK_applications
```
sudo pacman -S breeze breeze-gtk breeze-icons qt5ct lxappearance
```
* Config QT
   ```
   echo "QT_QPA_PLATFORMTHEME=qt5ct" >> /etc/environment
   ```
   Open ```qt5ct``` and config it.

* Config GTK

   Open ```lxappearance``` and config it.
