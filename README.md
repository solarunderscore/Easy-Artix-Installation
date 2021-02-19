# Easy Artix Installation
*A simple, quick, and easy Artix Installation guide.*
**This installation will be for a OpenRC init system.**

What is Artix Linux? Artix Linux is a Linux distubution that is based off the popular distrubtion Arch Linux. So what differs from Artix and Arch Linux? Arch uses the popular init system called "SystemD". SystemD isn't necessarily bad, the reason why most people want to use another init system besides SystemD is because of the "non-bloat" potential of the init system. SystemD doesn't have isolated containters/groups which each program can run in, so it doesn't follow the "UNIX Philosophy". There are other init systems such as OpenRC, s6, and runit.

Artix has multiple ways to install. You get to install it via a GUI like Manajaro or Ubuntu, while there is another way such as using a CLI (Command Line Interface). Today I will be installing it using the terminal for the minimal install so we can get as least bloat as possible.

## Prerequisites (Links are at the end of this section)
If you have followed my previous guide (Easy Arch Installation) this will be very similar to that installation.  
Before you do anything make sure you backup all your drives because there could be a chance of you wiping a drive you do not intend on wiping.

Things you will need to install this on a machine is a 4-8GB USB flash drive or if you're feeling really old timey you can get a CD Disk. Get this program called "balenaetcher", this will allow you to brun the ISO image to the USB or CD drive. Once, you have that program installed get the Artix Linux installation media. Make sure you are getting the minimal install which normally goes by the name *"artix-base-(init-system)-(date-of-release-x86_64.iso"* The GUI installs will include the desktop environment in the ISO name. For example, *"artix-(desktop-environment)-openrc-(date-of-release)-x86_64.iso"*. Once you have gotten your ISO, fire up balenaetcher and select your ISO. Then proceed to select the USB device and click "Flash!". **This will wipe everything on your USB so make sure to back everything up!**

*Links:*
Artix Linux ISO Download: https://iso.artixlinux.org/isos.php
balenaetcher Download (Windows, MacOS, Linux download): https://www.balena.io/etcher/

## Boot from the USB or CD Drive
Afterwards, you can reboot into your Boot Menu. Your Boot Menu button is different for every motherboard. For me, the Boot Menu button is F11. So I would reboot and press F11 to get to my boot menu, and then I would select the USB you just made.

## Pre-installation
### Select the correct timezone
Select the correct time zone using the arrow keys to the location that is nearest to you. The time zone section should be labeled *tz=*.

### Selecting the proper keytable
Make sure you select the proper keytable for this installation. The default keytable is the standard US QWERTY layout. Once you have selected your keytable you are ready for this installation.

### Booting from the proper installation
Now we will boot from the USB to install. Make sure that you select the right one. If you are using a USB Flash Drive you want to select *From Stick/HDD: artix.x86_64* and if you are booting from a CD/DVD device or a ISO file if you on a virtual machine you will select, *From CD/DVD/ISO: artix.x86_64*. This will boot you into the USB installation.

### Logging in as root
Logging in as root is pretty simple. The default username and password for Live CD account is `artix` for both the username and password. This logs you into the USB. To log in as root just type `su` to enter as root.

## Installation
### Verify the boot mode
For this guide I will be using a UEFI enabled BIOS. To check if your BIOS is UEFI enabled type the command:  
`ls /sys/firmware/efi/efivars`  
If you get some sort of output you are good for this guide. If you do not see any output or get an error this guide is irrelevant to you as there is some other steps and other requirements you most do inorder for your system to work.

### Making sure you have a network connection
Having a network connection is important so we can connect to servers and get the packages we need to install the system. For this installation guide I will be using Ethernet and if you are using WiFi you most run some other command to make sure you can connect to your local WiFi. To check if you have a internet connection use this,  
`ping -c 3 artixlinux.org` (This will send 3 packets to the server to verify a connection.)  
If you see lines saying `64 bytes from (ip address)` then you are good to go. You have a stable internet connection.

### Setting up the disks
To setup the disks make sure you have already backed everything up as you may wipe some drive you do not intend on wiping. To list and see all your drives run,  
`lsblk`  
and this shall list all your drives. Find the drive you want to install the system on and run,  
`cfdisk`  
This will open a CLI that will show how to partition the drives. Choose the label type as GPT and press enter.

Press ENTER on your *Free space* and make this around 200MB of storage for your EFI partition. To specify the amount of space type in "200M" and press ENTER. Go over to where it says *Type* and press ENTER. Scroll up until you are selecting EFI system and press ENTER. Now to create your swap.

Swap space on Linux is just backup space incase your root partition becomes to full. Swap space also correlates to the amount of RAM you have. In my system I have 16GB so I gave it 2GB. Now you want to make sure this is a swap partition by going over to *Type* and select `Linux swap`.

Create the rest of your free space to Linux filesystem and this will be your root/home partition. Afterward you can write and quit. **(After writing the disk it cannot be undone.)**
### Formatting the disks
To format the disks make sure you are formatting it correctly to the right partition. Do `cfdisk` again if you are unsure. To format your root/home partition we will do,  
`mkfs.ext4 /dev/root-partition`  
It is *sda3* for me because I am using a SATA SSD with the root/home partition being on the 3rd partition of my disk.

To make the swap and to turn it on do,  
`mkswap /dev/swap-partition`  
This should format it to swap and I chose *sda2* because my swap partition is located on `/dev/sda2`.  
Now to turn on the swap partition use,  
`swapon /dev/swap-partition`  
This will turn on the swap partition.

Format the EFI partiton by doing,  
`mkfs.fat -F32 /dev/efi-parition`  
This will format the EFI-partiton as FAT32.

Now you want to mount the root partition to the `/mnt` directory. To do this simply run the command,  
`mount /dev/root-partition /mnt`  
Now the disk is muonted.

Now to setup our EFI directory. The directory does not yet exist so run the command to make the directory,  
`mkdir /mnt/efi`  
This will make the directory for EFI partition.  
Mount your EFI partition to the new directory by doing.  
`mount /dev/efi-partition /mnt/efi`  
This will mount it to the `/mnt/efi` directory.

### Installing essential packages to make our system run
Now that we have setup our disks it is time to get packages to make our system run. This is quite similar to Arch, but with some slight modifications. Run the command,  
`basestrap /mnt base openrc linux linux-firmware` (This will install for OpenRC and not Runit or s6.)  
This will install the kernel and the base package with the init system.

### Generating the fstab  
The fstab is so when your system boots it can know which drives to mount on startup. So to generate this file do,  
`fstabgen -U /mnt >> /mnt/etc/fstab`  
You should not get an output here.

### Entering your new system
At last it is time to enter your new system. This is quite similar to Arch, but in stead of `arch-chroot /mnt` you will need to run,  
`artix-chroot /mnt` (This was formerly known as `artools-chroot`.)  
This should enter you into the default shell, you should type `bash` and press ENTER to get all of the bash shell tools.

## Seting up your new operating system
### Setting up your system clock
Time to set up your timezone for your system clock! To do this simply type:  
`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`  
But replace "Region" and "City" with the options they provided. Press TAB on your keyboard when you get to `/usr/share/zoneinfo/` to see all the options. *Example:`ln -sf /usr/share/zoneinfo/US/Eastern /etc/localtime`*

Now sync the hardware clock by doing  
`hwclock --systohc`

### Installing a console based text editor
For this installation I will be installing nano as my console based text editor. To do this I will run,  
`pacman -Sy nano`  
This will synchronize the mirrorlist and install nano.

### Configuring the locale
To configure our locale is to define your language and reigon. For my usage a am a English speaker so I will user the locale `en_US.UTF-8`.  
Use nano to open up your locale.gen file.  
`nano /etc/locale.gen`  
Once you are in this file find the second instance of `en_US.UTF-8 UTF-8` and uncomment that.

To generate your locale do,  
`locale-gen`

Now make a new locale configuration file by doing,  
`nano /etc/locale.conf`  
Now type in your locale you just uncomment. For me I would type `LANG=en_US.UTF-8` since I uncommented that.

### Get a network manager for you new system.  
The network manager I will be using for this is called *Network Manager*. To install it for OpenRC we would need to do,  
`pacman -S networkmanager networkmanager-openrc`  
This will install NetworkManager and the tools to use it.

Now we want to start NetworkManager at boot. So to do this run,  
`rc-update add NetworkManager`  
since this is a OpenRC system we will be using `rc-update` instead of `systemctl`

### Setting up the hostname and the host domain
To create your hostname run,  
`nano /etc/hostname`
type in whatever you want here and write the file out. The hostname is what other computers will see you as on the network.

Now we want to configure the host domain.  
To do this enter the file hosts with your text editor.  
`nano /etc/hosts`  
You should see some stuff already in here. Just go to the end and press ENTER. Now you can setup your host domain like so...  
*Before:*  
```
# Static table lookup for hostnames.  
# See hosts(5) for details.  

```
*After:*  
```
# Static table lookup for hostnames.
# See hosts(5) for details.  

127.0.0.1	localhost
::1		localhost
127.0.1.1	your-hostname.localdomain	your-hostname
```
Where it says `your-hostname` put the host name you chose in `/etc/hosts`. For `127.0.0.1` you can leave it as is or you can change it if you want a specific IP address linked to your domain. Now you can write the file and quit.

### Adding a user
To add a user run the command,  
`useradd -G wheel,audio,video -m username`  
Replace `username` with the name you want for your new user. This will create the new user and its home directory.

To setup a password for the new user, type:  
`passwd username`  
then enter a password of your choice and press ENTER, then confirm your password. This should update your password.

Now make sure your home directory for the user exists. To check if you have the directory run,  
`ls /home/`  
then press TAB and if your username appears then it is there! Now you can delete the line.

Now set a password for your root user by doing,  
`passwd`  
and enter in your password for your root.

### Installing a bootloader
For this guide I will be using the GRUB bootloader since it works for virtually any motherboard UEFI/BIOS out there. You will want to grab these packages for GRUB.  
`pacman -S grub efibootmgr` (You may want to grab the `os-prober` package too if you are dual booting.)  
This will install GRUB and efibootmgr.

Time to install GRUB to your /efi/ directory. Run this in your terminal,  
`grub-install --target=x86_64-efi --efi-directory=/efi/ --bootloader-id=GRUB`

Now we will create the GRUB configuration file. To do this run,  
`grub-mkconfig -o /boot/grub/grub.cfg`  
and it should output somethings and the config should be made!

### Entering your new system!
Congratulations! After that you are practically done! You can enter your new system by doing,  
`exit` (Do this twice to exit bash and shell.)  
Afterward you can type,  
`reboot`  
and remove the USB and you can boot into your new system!

## Finale
Congratulations! You have just installed Artix Linux! Right now it is just a terminal, you will need to setup a Desktop Environment by yourself. I have a guide for it in my Arch Linux Installation guide [here](https://github.com/solarunderscore/Easy-Arch-Installation#installing-a-desktop-environment).
