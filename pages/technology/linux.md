# Linux

## Installing Arch Linux

- [Arch Linux Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide)
- [Luke Smith Full Arch Linux Install](https://www.youtube.com/watch?v=4PBqpX0_UOc)

 1. Download `archlinux-YYYY.MM.DD-x86_64.iso` from [Arch Linux Downloads](https://www.archlinux.org/download/)
 2. Make bootable USB stick (see [Arch USB flash installation guide](https://wiki.archlinux.org/index.php/USB_flash_installation_media#In_Windows))
    - On Windows use Rufus
 3. Mount USB onto the target device and select `Boot Arch Linux (x64_64)`
 4. Type `ls /sys/firmware/efi/efivars` to check if you need UEFI
    - If there's no directory then continue
    - If there's files here you may need to do the UEFI install procedure
 5. Type `ping archlinux.org` to verify internet connection and type `ctrl+c` to stop pinging
 6. Type `timedatectl set-ntp true` to update the system clock by and type `timedatectl status` to check it
 7. Partition the drives (usually boot, swap, root, and home partitions)
    - Type `lsblk` or `fdisk -l` to see your disk devices
    - Type `fdisk /dev/sda`, you may type `m` for help
    - Type `p` to print out everything that is in the drive
    - Type `d` to delete partitions for each partition in the list
    - Create **Boot Partition**
        - Type `n` to create a new partition and type `p` for primary and type `1` to give it a number
        - Type `Enter` to keep first sector default
        - Type `+256M` to set the partition size to 256MB
        - If asked to remove the signature type `Y`
    - Create a **Swap Partition** (mostly used for computer hybernation)
        - Type `n` to create a new partition and type `p` for primary and type `2` to give it a number
        - Type `Enter` to keep first sector default
        - Type `+12G` to set the partition size to 12GB (The rule of thumb is 150% of RAM size.  My RAM was 8GB)
    - Create a **Root Partition** (where all the programs and packages are installed)
        - Type `n` to create a new partition and type `p` for primary and type `3` to give it a number
        - Type `Enter` to keep first sector default
        - Type `+24G` to set the partition size to 24GB
    - Create a **Home Partition** (your files)
        - Type `n` to create a new partition and type `p` for primary and type `4` to give it a number
        - Type `Enter` to keep first sector default
        - Type `Enter` to fill up the last sector will the remaining space
    - Type `p` to print everything. DOUBLE CHECK BEFORE MOVING ON!
    - Type `w` to write
    - Type `lsblk` to see your disk devices
 8. Make the file systems
    - Type `mkfs.ext4 /dev/sda1` for boot partition
    - Type `mkfs.ext4 /dev/sda3` for root partition
    - Type `mkfs.ext4 /dev/sda4` for home partition
 9. Type `mkswap /dev/sda2` to make the swap partition and then type `swapon /dev/sda2`
10. Mount the file systems
    - Type `mount /dev/sda3 /mnt` to mount the root partition
    - Type `mkdir /mnt/boot` to create the boot directory
    - Type `mkdir /mnt/home` to create the home directory
    - Type `ls /mnt` and you should see "boot home lost+found"
    - Type `mount /dev/sda1 /mnt/boot` to mount the boot directory
    - Type `mount /dev/sda4 /mnt/home` to mount the home directory
11. Type `pacstrap /mnt base base-devel vim` to install Arch
12. Generate an fstab file
    - Type `genfstab -U /mnt >> /mnt/etc/fstab`
    - Type `vim /mnt/etc/fstab` to check it
13. Type `arch-chroot /mnt` to navigate out of the usb arch linux and into the computer's root directory.  Verify by typing `ls`
14. Install network manager
    - Type `pacman -S networkmanager` to install it
    - Type `systemctl enable NetworkManager` to automatically start it on boot
15. Install boot loader
    - Type `pacman -S grub` to install it
    - Type `grub-install --target=i386-pc /dev/sda` to install grub on your machine
    - Type `grub-mkconfig -o /boot/grub/grub.cfg` to make the configuration file
16. Type `passwd` to set the root password
17. Generate the locale
    - Type `vim /etc/locale.gen`
    - In vim type `/en_US`, uncomment `en_US.UTF-8 UTF-8` and `en_US ISO-8859-1`, and type `:x` to save and exit
    - Type `locale-gen` to generate the locales
    - Type `vim /etc/locale.conf` to create the configuration file, put `LANG=en_US.UTF-8` in it, and save+exit
18. Set the time zone
    - Type `ls /usr/share/zoneinfo` to browse the time zone options
    - Type `ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime` to link the localtime file to your time zone
    - Type `date` and verify the date and time
19. Type `vim /etc/hostname`, type the name for the computer, and save+exit
20. Type `exit` to leave the computer's root directory and back to the usb arch linux directory
21. Type `umount -R /mnt` to unmount all partitions and type `lsblk` to verify
21. Type `shutdown now` to shutdown the computer, remove the usb drive, then start the computer

## Installing VirtualBox for Windows Virtual Machines

- [Installing Arch Linux in VirtualBox](https://www.youtube.com/watch?v=HpskN_jKyhc)

 1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
 2. Create a new Virtual Machine
    - Name: Arch Linux
    - Type: Linux
    - Version: Arch Linux (64-bit)
 3. Memory size: 4096MB (4GB)
 4. Select `Create a virtual hard disk now`
 5. Select `VDI (VirtualBox Disk Image`
 6. Select `Dynamically allocated`
 7. File Size: 16.00GB (usually 16GB is more than enough)
 8. Select `Create`
 9. Select `Settings...`
10. Under `System` set `Processor` to 2 cores
11. Under `Display` set
    - Video Memory: 64MB
    - Graphics Controller: VBoxVGA
    - Acceleration: Enable 3D Acceleration
12. Under `Storage` under `Controller: IDE` add a new optical drive and navigate to the `archlinux-YYYY.MM.DD-x86_64.iso` (Note: see Arch Linux install for download details)
13. Start the VM and go through Arch Linux installation
14. Turn off VM and detatch the .iso from the Controller: IDE
15. Start VM!
16. Install VirtualBox Guest Additions (better screen resizing):
    - Type `pacman -S virtualbox-guest-utils xf86-video-vmware` and make sure you select `virtualbox-guest-modules-arch`
    - Configure to run on startup by typing `systemctl enable vboxservice.service`
    - Type `reboot`

## Adding Users

- [After a Minimal Linux](https://www.youtube.com/watch?v=nSHOb8YU9Gw)
- [Arch Wiki Users and Groups](https://wiki.archlinux.org/index.php/users_and_groups)

1. Add a user by typing `useradd -m -g wheel [username]`
2. Add a password by typing `passwd [username]`
3. Give your user sudo access by editing `/etc/sudoers`

### Packages

- <https://lukesmith.xyz/programs>
- [LukeSmithxyz/LARBS/progs.csv](https://github.com/LukeSmithxyz/LARBS/blob/master/progs.csv)

| Package  | Description |
|----------|-------------|
|          |             |

## Window Manager

- [After a Minimal Linux](https://www.youtube.com/watch?v=nSHOb8YU9Gw)

1. Install xorg packages: `pacman -S xorg-server xorg-xinit`
2. Install i3-gaps and other related useful packages: `pacman -S i3-gaps i3status rxvt-unicode dmenu network-manager-applet`
3. In `~/.xinitrc` put `exec i3`
4. (If using VirtualBox) in `~/.config/i3/config` put `exec VBoxClient-all`
5. Type `reboot`
6. Login and type `startx`
7. Open terminal with `Mod+ENTER`

## Fonts

- [After a Minimal Linux](https://www.youtube.com/watch?v=nSHOb8YU9Gw)

1. Install fonts by typing `pacman -S ttf-linux-libertine ttf-inconsolata`
2. (Maybe not necessary) Set fonts manually in `vim ~/.config/fontconfig/fonts.conf`

