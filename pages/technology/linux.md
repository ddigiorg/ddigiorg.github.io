# Linux

## Installation

### VirtualBox

 1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
 2. Create a new Virtual Machine
    * Name: Arch Linux
    * Type: Linux
    * Version: Arch Linux (64-bit)
 3. Memory size: 4096MB (4GB)
 4. Select `Create a virtual hard disk now`
 5. Select `VDI (VirtualBox Disk Image`
 6. Select `Dynamically allocated`
 7. File Size: 16.00GB (usually 16GB is more than enough)
 8. Select `Create`
 9. Select `Settings...`
10. Under `System` set `Processor` to 2 cores
11. Under `Display` set
    * Video Memory: 64MB
    * Graphics Controller: VBoxVGA
    * Acceleration: Enable 3D Acceleration
12. Under `Storage` under `Controller: IDE` add a new optical drive and navigate to the `archlinux-YYYY.MM.DD-x86_64.iso` (Note: see Arch Linux install for download details)
13. Start the VM and go through Arch Linux installation
14. Turn off VM and detatch the .iso from the Controller: IDE
15. Start VM!
16. Install VirtualBox Guest Additions (better screen resizing):
    * Type `pacman -S virtualbox-guest-utils xf86-video-vmware` and make sure you select `virtualbox-guest-modules-arch`
    * Configure to run on startup by typing `systemctl enable vboxservice.service`
    * Type `reboot`

### Arch Linux

* [Arch Linux Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide)
* [Luke Smith Full Arch Linux Install](https://www.youtube.com/watch?v=4PBqpX0_UOc)
* [Installing Arch Linux in VirtualBox](https://www.youtube.com/watch?v=HpskN_jKyhc)

 1. Download `archlinux-YYYY.MM.DD-x86_64.iso` from [Arch Linux Downloads](https://www.archlinux.org/download/)
 2. Make bootable USB stick (see [Arch USB flash installation guide](https://wiki.archlinux.org/index.php/USB_flash_installation_media#In_Windows)
    * On Windows use Rufus
 3. Mount USB onto the target device and select `Boot Arch Linux (x64_64)`
 4. Verify internet connection by typing `ping archlinux.org` then use `ctrl+c` to stop pinging
    * If no ethernet connection
 5. Update the system clock by typeing `timedatectl set-ntp true` then check it with `timedatectl status`
 6. Partition the disks (usually boot, swap, root, and home partitions)
    * Type `lsblk` or `fdisk -l` to see your disk devices
    * Modify partition tables using `cfdisk` and select `dos`
    * Select `[New]` then `[primary]`
    * Select `[Bootable]`
    * Select `[Type]` as `Linux`
    * Select `[Write]` then `yes`
    * Note: swap partition not necessary for VM
    * select `[Quit]`
 7. Make a file system by typing `mkfs.ext4 /dev/sda1`
 8. Mount the file systems by typing `mount /dev/sda1 /mnt`
 9. You can edit your mirrorlist by typing `vim /etc/pacman.d/mirrorlist`
10. Install base Arch system by typing `pacstrap /mnt base base-devel vim`
11. Generate an fstab file by typing `genfstab -U /mnt >> /mnt/etc/fstab`. You can check it by typing `vim /mnt/etc/fstab`
12. Change root into the new system: `arch-chroot /mnt`
13. Set the time zone: `ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime` then `hwclock --systohc`
14. Edit localization
    * Uncomment `en``_US.UTF-8 UTF-8_` _and_ `_en__``US ISO-8859-1` in `/etc/locale.gen` (i.e. using vim: `vim /etc/locale.gen`)
    * Generate locales by typing `locale-gen`
    * Type `vim /etc/locale.conf` to create the file and put `LANG=en_US.UTF-8` in it
15. Edit the network config:
    * Create the hostname file by typing `vim /etc/hostname` and put in it whatever you want the hostname to be like `archvm`
    * Create a hosts file by typeing `vim /etc/hosts` and put in it:

          127.0.0.1	localhost
          ::1		localhost
          127.0.1.1	myhostname.localdomain	myhostname
    * Install a network manager by typing `pacman -S networkmanager`
    * Configure to run on startup by typing `systemctl enable NetworkManager`
16. Set root password by typeing `passwd`
17. Install boot loader
    * Get grub by typing `pacman -S grub`
    * Install grub by typing `grub-install --target=i386-pc /dev/sda`
    * Save configuration file `grub-mkconfig -o /boot/grub/grub.cfg`
18. Exit chroot environment by typing `exit`
19. Unmount the partitions with `umount -R /mnt`
20. Type `reboot  !!! need to unmount iso`

### LARBS

- [LukeSmithxyz/LARBS](https://github.com/LukeSmithxyz/LARBS)

Installing packagas and configs via Luke's Auto-Rice Bootstraping Scripts (LARBS):

curl -LO larbs.xyz/larbs.sh
sh larbs.sh

### Users

* [After a Minimal Linux](https://www.youtube.com/watch?v=nSHOb8YU9Gw)
* [Arch Wiki Users and Groups](https://wiki.archlinux.org/index.php/users_and_groups)

1. Add a user by typing `useradd -m -g wheel [username]`
2. Add a password by typing `passwd [username]`
3. Give your user sudo access by editing `/etc/sudoers`

### Window Manager

* [After a Minimal Linux](https://www.youtube.com/watch?v=nSHOb8YU9Gw)

1. Install xorg packages: `pacman -S xorg-server xorg-xinit`
2. Install i3-gaps and other related useful packages: `pacman -S i3-gaps i3status rxvt-unicode dmenu network-manager-applet`
3. In `~/.xinitrc` put `exec i3`
4. (If using VirtualBox) in `~/.config/i3/config` put `exec VBoxClient-all`
5. Type `reboot`
6. Login and type `startx`
7. Open terminal with `Mod+ENTER`

### Fonts

* [After a Minimal Linux](https://www.youtube.com/watch?v=nSHOb8YU9Gw)

1. Install fonts by typing `pacman -S ttf-linux-libertine ttf-inconsolata`
2. (Maybe not necessary) Set fonts manually in `vim ~/.config/fontconfig/fonts.conf`