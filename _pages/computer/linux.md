---
layout: page
title: Linux
date: 2021-08-08
tags: computer linux
---

## Installing Arch Linux

[Arch Linux Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide)

 1. **Boot Arch USB**
    - Download `archlinux-YYYY.MM.DD-x86_64.iso` from [Arch Linux Downloads](https://www.archlinux.org/download/)
    - Make bootable USB stick by following the [Arch USB flash installation guide](https://wiki.archlinux.org/index.php/USB_flash_installation_media#In_Windows)
        - On Windows use Rufus
    - Mount USB onto the target device and select `Boot Arch Linux (x64_64)`
 2. **Verify Boot Mode**
    - Type `ls /sys/firmware/efi/efivars` to check if you need UEFI
        - If there's no directory then continue
        - If there's files here you may need to do the UEFI install procedure
 3. **Connect to Internet**
    - Type `ip link` and verify the network intterface is listed and enabled
    - Type `ping archlinux.org` to verify internet connection
    - Type `ctrl+c` to stop pinging
 4. **Update System Clock**
    - Type `timedatectl set-ntp true` to update the system clock
    - Type `timedatectl status` to check it
 5. **Partition Disks**
    - Type `lsblk` or `fdisk -l` to see your disk devices
    - Type `fdisk /dev/sda`, you may type `m` for help
    - Type `p` to print out everything that is in the drive
    - Type `d` to delete partitions for each partition in the list
    - **Boot Partition**
        - Type `n` to create a new partition, `p` for primary, and `1`
        - Type `Enter` to keep first sector default
        - Type `+256M` to set the partition size to 256MB
        - If asked to remove the signature type `Y`
    - **Swap Partition** (mostly used for computer hybernation)
        - Type `n` to create a new partition, `p` for primary, and `2`
        - Type `Enter` to keep first sector default
        - Type `+8G` to set the partition size to 8GB (Rule of thumb is 1x RAM size)
    - **Root Partition** (where all the programs and packages are installed)
        - Type `n` to create a new partition, `p` for primary, and `3`
        - Type `Enter` to keep first sector default
        - Type `+24G` to set the partition size to 24GB
    - **Home Partition** (your files)
        - Type `n` to create a new partition, `p` for primary, and `4`
        - Type `Enter` to keep first sector default
        - Type `Enter` to fill up the last sector will the remaining space
    - Type `p` to print everything. DOUBLE CHECK BEFORE MOVING ON!
    - Type `w` to write
    - Type `lsblk` to see your disk devices
 6. **Format Partitions**
    - Type `mkfs.ext4 /dev/sda1` for boot partition
    - Type `mkfs.ext4 /dev/sda3` for root partition
    - Type `mkfs.ext4 /dev/sda4` for home partition
    - Type `mkswap /dev/sda2` to make the swap partition
    - Type `swapon /dev/sda2` to initialize it
 7. **Mount File Systems**
    - Type `mount /dev/sda3 /mnt` to mount the root partition
    - Type `mkdir /mnt/boot` to create the boot directory
    - Type `mkdir /mnt/home` to create the home directory
    - Type `ls /mnt` and you should see "boot home lost+found"
    - Type `mount /dev/sda1 /mnt/boot` to mount the boot directory
    - Type `mount /dev/sda4 /mnt/home` to mount the home directory
 8. **Install Essential Packages**
    - Type `pacstrap /mnt base base-devel linux linux-firmware vim`
 9. **Fstab**
    - Type `genfstab -U /mnt >> /mnt/etc/fstab` to generate an fstab file
    - Type `vim /mnt/etc/fstab` to check it
10. **Chroot**
    - Type `arch-chroot /mnt` to navigate out of the usb arch linux and into the computer's root directory.
    - Type `ls` to verify
11. **Time Zone**
    - Type `ls /usr/share/zoneinfo` to browse the time zone options
    - Type `ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime` to link the localtime file to your time zone
    - Type `date` and verify the date and time
    - Type `hwclock --systohc`
12. **Localization**
    - Type `vim /etc/locale.gen`
    - Uncomment `en_US.UTF-8 UTF-8` and `en_US ISO-8859-1`
    - Type `locale-gen` to generate the locales
    - Type `vim /etc/locale.conf` to create the configuration file and write `LANG=en_US.UTF-8`
13. **Network Configuration**
    - Type `vim /etc/hostname` and write in a hostname
    - Type `vim /etc/hosts` and add:
        ```
        127.0.0.1    localhost
        ::1	       localhost
        127.0.1.1    myhostname.localdomain    myhostname
        ```
    - Type `pacman -S networkmanager` to install it
    - Type `systemctl enable NetworkManager` to automatically start it on boot
14. **Root Password**
    - Type `passwd` to set the root password
15. **Boot Loader**
    - Type `pacman -S grub` to install it
    - Type `grub-install --target=i386-pc /dev/sda` to install grub on your machine
    - Type `grub-mkconfig -o /boot/grub/grub.cfg` to make the configuration file
16. **Cleanup**
    - Type `exit` to leave the computer's root directory and back to the usb arch linux directory
    - Type `umount -R /mnt` to unmount all partitions and type `lsblk` to verify
    - Type `shutdown now` to shutdown the computer, remove the usb drive, then start the computer
17. **Login**
    - Start up the computer and log in to root
18. I reccomend from here to directly install LARBS

## Luke's Auto-Rice Bootstrapping Scripts (LARBS)

Installing or reinstalling(or updating) LARBS:

- Type `sudo su` to ensure you are super user
- Type `cd ~` to navigate too the `root/` directory
- Type `pacman -Syu` to ensure all the arch packages are up to date
- Type `curl -LO larbs.xyz/larbs.sh` to get the larbs shell script
- Type `sh larbs.sh` and follow the promps

Notes:

- [LARBS](https://github.com/lukesmithxyz/larbs)
- [dotfiles](https://github.com/lukesmithxyz/voidrice)
- [A Friendly Guide to LARBS](https://larbs.xyz/dwm.pdf)
- [Luke Smith](https://lukesmith.xyz), innawoods unaboomer

## Definitions

| Definition         | Description |
|--------------------|-------------|
| `kernel`           | A program at the core of a computer's operating system and has complete control over everything in the system (e.g. Linux). |
| `operating system` | Software that manages computer hardware, software resources, and provides common services for computer programs (e.g GNU, Ubuntu, Arch, etc.). |
| `terminal`         | Device or emulator used to execute commands to interact with the computer. |
| `shell`            | The user interface used to interact with the linux kernel and operating system (e.g. sh, bash). |
| `$`                | Indicates you are logged in as a user. |
| `#`                | Indicates you are logged in as root. |
| `root`             | The superuser account with all administrative capabilities. |
| `user`             | An entity that can perform operations. |
| `group`            | A collection of users. |
| `package`          | A collection of files that perform a task. |
| `package manager`  | A program that manages packages (e.g. dpkg, apt, pacman). |
| `repository`       | A storage location (typically a server) that has a collection of software. |
| `daemon`           | A long-running backgroung process that answers requests for services. |
| `process`          | An instance of a running program. |

## Commands

Helpful tips:

- The [Core Utilities](https://wiki.archlinux.org/index.php/Core_utilities) are the basic, fundamental tools of a GNU/Linux system
- Commands are stored in files called "binaries"
- Type `compgen -c` to list all commands that could be executed
- Type `man [command]` to print the user manual for that command
- Type `[command] -h (or --help)` to also view information about that command
- Type `apropos [keyword]` to search the manuals for that keyword
- Type `ctrl+a` to get to the front of your command (useful if you forgot to type sudo in front of a command)
- Use `uparrow` to get previous commands
- Use `&&` between commands to execute them one one line in the terminal
- Use `|` between commands to pipe, or send output, of one command to another for further processing

| Command    | Description                                       |
|------------|---------------------------------------------------|
| `compgen`  | list all the commands that could be executed      |
| `man`      | an interface to the system reference manuals      |
| `apropos`  | search the manual page names and descriptions     |
| `cat`      | concatenate files and print on stdout (or shell)  |
| `pwd`      | print workind directory                           |
| `ls`       | list directory                                    |
| `cd`       | change directory                                  |
| `whoami`   | who am I (prints current user name)               |
| `clear`    | clear terminal screen                             |
| `mkdir`    | make directory                                    |
| `rmdir`    | remove empty directory                            |
| `rm`       | remove files or directories                       |
| `cp`       | copy files or directories                         |
| `mv`       | move files or directories                         |
| `ln`       | make hard or symbolic links                       |
| `chown`    | change file owner and group                       |
| `chmod`    | change file permissions                           |
| `dd`       | convert and copy a file                           |
| `df`       | report file system disk space usage               |
| `tar`      | tar archiver                                      |
| `less`     | terminal pager                                    |
| `find`     | search files or directories                       |
| `diff`     | compare files line by line                        |
| `grep`     | print lines matching a pattern                    |
| `sed`      | stream editor                                     |
| `awk`      | pattern scanning and processing language          |
| `dmesg`    | print or control the kernel ring buffer           |
| `lsblk`    | list block devices                                |
| `mount`    | mount a filesystem                                |
| `umount`   | unmount a filesystem                              |
| `su`       | substitute user                                   |
| `sudo`     | superuser do                                      |
| `exit`     | cause normal process termination                  |
| `kill`     | terminate a process                               |
| `pgrep`    | look up processes by name or attributes           |
| `ps`       | report a snapshot of the current processes        |
| `pstree`   | shows running processes as a tree                 |
| `free`     | display amount of free and used memory            |
| `which`    | identify the location of executables              |
| `id`       | print real and effective user and group IDs       |
| `uname`    | print system information                          |
| `ip`       | print network information                         |
| `netstat`  | print status of the network                       |
| `ifconfig` | configure a network interface                     |
| `who`      | show who is logged on                             |
| `env`      | print environment variables                       |

## The Filesystem Hierarchy Standard (FHS)

The [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) (FHS) defines the directory structure and directory contents in Linux distributions.  Everything in linux is represented by a file including devices, commands, and settings.

| Directory     | Description                                                                                                    |
|---------------|----------------------------------------------------------------------------------------------------------------|
| `/`           | Root directory of entire filesystem hierarchy.                                                                 |
| `/bin`        | Essential command binaries that need to be available in single user mode; for all users, e.g., cat, ls, cp.    |
| `/boot`       | Boot loader files, e.g., kernels, initrd.                                                                      |
| `/dev`        | Device files.                                                                                                  |
| `/etc`        | Host-specific system-wide configuration files.                                                                 |
| `/home`       | Users' home directories, containing saved files, personal settings, etc.                                       |
| `/lib`        | Libraries essential for the binaries in `/bin` and `/sbin`.                                                    |
| `/lib64`      | Alternative format essential libraries.                                                                        |
| `/lost+found` | Orphaned or corrupted files are places here.                                                                   |
| `/mnt`        | Temporarily mounted filesystems. (or /media)                                                                   |
| `/opt`        | Optional application software packages.                                                                        |
| `/proc`       | Virtual filesystem providing process and kernel information as files.                                          |
| `/root`       | Home directory for the root user.                                                                              |
| `/run`        | Run-time variable data.                                                                                        |
| `/sbin`       | Essential system binaries, e.g., fsck, init, route.                                                            |
| `/srv`        | Site-specific data served by this system.                                                                      |
| `/sys`        | Contains information about devices, drivers, and some kernel features.                                         |
| `/tmp`        | Temporary files. (or /temp)                                                                                    |
| `/usr`        | Secondary hierarchy for read-only user data; contains the majority of (multi-)user utilities and applications. |
| `/var`        | Variable files—files whose content is expected to continually change during normal operation of the system.    |

## Managing Users

[Users and Groups](https://wiki.archlinux.org/index.php/users_and_groups) are used on GNU/Linux for access control to system files, directories, and peripherals.

Adding users:

1. Type `sudo useradd -m [username]` to add a user (-m creates home directory)
2. Type `sudo passwd [username]` to add a password
3. Type `cat /etc/passwd` to view the user file: `[username]:x:[user_id]:[group_id]:[name_and_comments]:[home_dir]:[default_shell]`

Adding a group:

1 Type `sudo groupadd [groupname]` 
2. (optional) Type `sudo gpasswd [groupname]` to add a password
3. Type `cat /etc/group` to view the group file: `[groupname]:x:[group_id]

Modifying user account:

- Use `usermod` (to view capabilities type `man usermod`)

Giving a user admin privileges:

1. Ensure you are root (or type `sudo` before commands)
2. (optional) Type `export EDITOR=vim` to set vim as the visudo editor instead of nano
3. Type `visudo` to properly edit `/etc/sudoers`
4. Under `User privilege specification` type `[username] All=(All) All`
5. Type `ctrl+x` to exit out of the file if using nano
6. Type `reboot` to restart computer
7. Login as the user
8. Type `sudo ls` to test if user has sudo access

Deleting users or groups:

- Type `sudo userdel [username]` to delete user
- Type `sudo groupdel [groupname]` to delete a group

## Managing Packages using Pacman (for Arch Linux)

[Pacman](https://wiki.archlinux.org/index.php/pacman) is an Arch Linux package management utility that tracks installed packages.  It should already be installed through the base package.

| Command               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `pacman -S [package]` | install package                                                             |
| `pacman -Syu`         | update package database and upgrade installed packages                      |
| `pacman -Sy`          | update package database                                                     |
| `pacman -Su`          | upgrade installed packages                                                  |
| `pacman -Syy`         | force update of package database even if recently updated                   |
| `pacman -Syyuw`       | download programs but leave them uninstalled (for manual install)           |
| `pacman -R [package]` | remove package                                                              |
| `pacman -Rs`          | remove package as well as unneeded dependencies                             |
| `pacman -Rns`         | remove package, dependencies, and system config files (reccomended)         |
| `pacman -Q`           | display all installed packages                                              |
| `pacman -Q | wc -l`   | display total number of installed packages by countint lines of output      |
| `pacman -Qe`          | display only packaged explicitly installed                                  |
| `pacman -Qeq`         | display only names of explicitly installed packages and not version numbers |
| `pacman -Qn`          | display only packages installed from main repositories                      |
| `pacman -Qm`          | display only packages installed from AUR                                    |
| `pacman -Qdt`         | display orphaned dependencies                                               |
| `pacman -Ss`          | search remote repository for package                                        |
| `pacman -Qs`          | search local repository for package                                         |

## Managing Daemons, Processes, and Services

- Note: may need to enable sudo for these commands

Listing processes:

- Note: Daemons will have a "d" at the end of the process name (e.g. "sshd" for the ssh daemon)
- Note: `systemd` is the "master daemon" or a system and service manager.  When linux boots, systemd is the first service started and controls mounting the file system and starting all the services.  Also note that systemd calls services or daemons "units".
- Type `ps -aux` to see every process on the system (remember a **process** is an instance of a **running** program.
- Type `ps -aux | grep [process]` to filter out all but the particular process
- Type `pstree` to see every process as a tree

Using `systemctl` to control services:

- Note: systemctl is a utility responsible for examining and controlling the systemd system and service manager
- Type `systemctl status [service]` to view the status of a service
- Type `systemctl is-active [service]` to view if the service is currently active
- Type `systemctl is-enabled [service]` to view if the service is currently enabled to start at boot
- Type `systemctl start [service]` to start a service
- Type `systemctl stop [service]` to stop a service
- Type `systemctl restart [service]` to restart a service
- Type `systemctl reload [service]` to reload a service
- Type `systemctl enable [service]` to start a service at boot
- Type `systemctl disable [service]` to not start a service at boot
- Type `systemctl list-units` to list all active (in memory) services, devices, et. al. (use `-t service` to only list services)
- Type `systemctl list-units | grep [service]` to list the particular service
- Type `systemctl list-unit-files` to list all services, devices, et. al.

## Man

[Man](https://wiki.archlinux.org/index.php/Man_page) lets you read package manuals.

1. Type `sudo pacman -S man-db` to install
2. Type `man [package]` to view the package's manual

## Yay

[Yay](https://github.com/Jguer/yay) or Yet Another Yogurt is a n AUR Helper Written in Go.

1. Type `git clone https://aur.archlinux.org/yay.git` to clone the yay repo
2. Type `cd yay`
3. Type `makepkg -si`
4. Type `yay --version` to verify install

## Git

[Git](https://wiki.archlinux.org/index.php/git) is the version control system (VCS) designed and developed by Linus Torvalds, the creator of the Linux kernel.

1. Type `sudo pacman -S git` to install git
2. Type `mkdir ~/.config/git` to create a git config directory
3. Type `git config --global user.name John Doe`
4. Type `git config --global user.email johndoe@example.com`
5. Type `git config --list` to check the user git settings
6. Type `git clone https://github.com/ddigiorg/ddigiorg.github.io.git` to clone ddigiorg's website

## Pip

[Pip](https://pip.pypa.io/en/stable/) is the package installer for Python.

- Type `pip install [package]` to install a particular Python package (might need to use pip3 to specify Python3)
- Type `pip install -r requirements.txt` to install the Python packages listed in the requirements.txt file

## Make

[Make](https://en.wikipedia.org/wiki/Make_(software)) is a build automation tool that automatically builds executable programs and libraries from source code by reading files called Makefiles which specify how to derive the target program.

1. Type `sudo pacman -S make` to install make
2. Type `make --version` to verify installation

## Xorg

[Xorg](https://wiki.archlinux.org/index.php/Xorg) (commonly referred as simply X) is the most popular display server among Linux users.

1. Type `sudo pacman -S xorg-server xorg-xinit xterm` to install X, xinit, and xterm
2. Type `lspci | grep -e VGA -e 3D` to identify the graphics card
3. Type `sudo pacman -Ss xf86-video` to search the remote package database for a list of open-source video drivers
4. Type `sudo pacman -S [driver package]` to install the driver (My ThinkPad x220 uses `xf86-video-intel`)
5. Type `startx` to test
6. Type `exit` in left terminal to exit
7. Type `cp etc/X11/xinit/xinitrc .xinitrc` to copy root xinit config file to user

- `.config/Xresources`
- `!xrdb .config/Xresources`

## Fonts

1. Type `pacman -S ttf-linux-libertine ttf-liberation` to install these fonts

## Brave

[Brave](https://brave.com/) is a web browser that blcoks ads and trackers by default.

1. Type `yay -S brave-bin` to install brave
2. Type `brave --version` to verify install
3. Type `brave` to launch brave

## DWM

[DWM](https://dwm.suckless.org/) is a dynamic window manager for X.

1. Type `cd ~/.local/src` to navigate to the appropriate directory
2. Type `git clone https://git.suckless.org/dwm` to clone the dwm repo
3. Type `sudo vim config.mk` and set:
    ```
    X11INC = /usr/include/X11
    X11LIB = /usr/lib/X11
    ```
4. Type `sudo make clean install` to install dwm
5. In `~/.xinitrc` add `exec dwm`

## ST

[ST](https://st.suckless.org/) is a simple terminal implementation for X.

1. Type `cd ~/.local/src` to navigate to the appropriate directory
2. Type `git clone https://git.suckless.org/st` to clone st repo
3. Type `sudo make clean install` to install st

- Default keybinding for opening up terminal is `Alt+Shift+Enter`

## SXHKD

[SXHKD](https://wiki.archlinux.org/index.php/Sxhkd) is a simple X hotkey daemon that reacts to input events by executing commands.

1. Type `sudo pacman -S sxhkd` to install sxhkd
2. Type `vim ~/.config/sxhkd/sxhkdrc` to edit the config file
3. In `~/.xinitrc` put `export XDG_CONFIG_HOME="$HOME/.config"` and `sxhkd &`
3. Type `systemctl --user restart sxhkd` to reload the user's sxhkd service

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

## Tutorials

NetworkChuck's tutorials (for complete newbies):

- [Linux for Hackers EP 1](https://www.youtube.com/watch?v=VbEx7B_PTOE)
- [Linux for Hackers EP 2](https://www.youtube.com/watch?v=A3G-3hp88mo)
- [Linux for Hackers EP 3](https://www.youtube.com/watch?v=Y17KTiJLcyQ)
- [Linux for Hackers EP 4](https://www.youtube.com/watch?v=jwnvKOjmtEA)
- [Linux for Hackers EP 5](https://www.youtube.com/watch?v=vX3krP6JmOY)
- [Linux for Hackers EP 6](https://www.youtube.com/watch?v=wOWhfNB_r-0)
