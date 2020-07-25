Linux Cheat Sheet Commands

Files
ls - List directory contents
cd [dir] - Change the working directory to [dir]
cd - Change the working directory to ~
cp [source] [destination] - Copies files/folders to destination
mv [source] [destination] - Moves files/folders to destination
mv [file] [new name] - Rename [file]
rm -rf [folder] - Force deletes [folder] and its contents
rmdir [folder] - Deletes folder
ln -s [file] [link] - Create symbolic [link] to [file]
nano [file] - Edit txt file
cat [file] - Output file content
rm -rf ~/.local/share/Trash/* - Empties trah folder. use sudo if needed
tar -xvf [file] - Extract [file]
makepkg -sri - Install package from inside the folder

System
udisksctl mount -b /dev/sd**  - Mount partition as user
udisksctl unmount -b /dev/sd**  - Unmount partition
sudo nano /etc/default/grub - Change GRUB settings
sudo grub-mkconfig -o /boot/grub/grub.cfg - Generate the main configuration file
sudo awk -F\' '/menuentry / {print $2}' /boot/grub/grub.cfg - Check grub entry list
id - Active user details
last - Shows the last logins in the system
who - Shows who is logged in to the system
groupadd [usrs] - Adds the group [usrs]
adduser [usr] - Adds user usr
userdel [usr] - Deletes user usr
usermod - Used for changing / modifying user information

System Information
whoami - Current user
uname -a - kernel version
lshw - system information
sudo dmidecode -t - Read DMI tables
lsblk - Check partions
sudo blkid - Get UUID of all partitions
findmnt - Check mounts on system
lsusb - Check USB devices
dmesg -wH - Watch kernel operation messages
inxi -F - Command line system information script for console and IRC
neofetch - A fast, highly customizable system info script
htop - Interactive process viewer
sudo fdisk -l | grep swap | cut -c1-9 - Get all swap filesystems

Network
ping [host] - Ping's host
whois [domain] - Get information on [domain]
ip addr show    - Show ip's

apt
sudo apt update - Checks for updates
apt list --upgradable - See the updates
sudo apt upgrade - Apply pending updates
sudo apt install [program] - Install [program]
sudo apt -f install - Fix dependencies
sudo apt autoremove [program] - Unninstall [program]
sudo apt autoremove - Removes old dependencies
sudo apt purge [program] - Purges all files related to [program]

pacman
sudo pacman -S [program] - Install [program]
sudo pacman -Sy - Checks for updates
sudo pacman -Syu - Apply pending updates
sudo pacman -Qi [program] - Check info on installed [program]
sudo pacman -R [Program] - Remove [program]
sudo pacman -Rcns [Program] - Remove all dependencies of [program]
sudo pacman -Qdtq - Check dependencies not used
sudo pacman -R $(pacman -Qdtq) - Remove dependencies not used
sudo ls /var/cache/pacman/pkg/ | wc -l - Check number of cached packages
du -sh /var/cache/pacman/pkg/ - Check size of cached packages
paccache -r - Deletes all cached versions of installed and uninstalled packages, except for the most recent 3
paccache -rk1 - To retain only one past version

pkcon
pkcon refresh - Checks for updates
pkcon update - Apply pending updates

