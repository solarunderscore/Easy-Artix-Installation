# Easy Artix Installation
*A simple, quick, and easy Artix Installation guide.*

What is Artix Linux? Artix Linux is a Linux distubution that is based off the popular distrubtion Arch Linux. So what differs from Artix and Arch Linux? Arch uses the popular init system called "SystemD". SystemD isn't necessarily bad, the reason why most people want to use another init system besides SystemD is because of the "non-bloat" potential of the init system. SystemD doesn't have isolated containters/groups which each program can run in, so it doesn't follow the "UNIX Philosophy". There are other init systems such as OpenRC, s6, and runit.

Artix has multiple ways to install. You get to install it via a GUI like Manajaro or Ubuntu, while there is another way such as using a CLI (Command Line Interface). Today I will be installing it using the terminal for the minimal install so we can get as least bloat as possible.

## Prerequisites (Links are at the end of this section)
If you have followed my previous guide (Easy Arch Installation) this will be very similar to that installation.  
Before you do anything make sure you backup all your drives because there could be a chance of you whiping a drive you do not intend on whiping.

Things you will need to install this on a machine is a 4-8GB USB flash drive or if you're feeling really old timey you can get a CD Disk. Get this program called "balenaetcher", this will allow you to brun the ISO image to the USB or CD drive. Once, you have that program installed get the Artix Linux installation media. Make sure you are getting the minimal install which normally goes by the name *"artix-base-(init-system)-(date-of-release-x86_64.iso"* The GUI installs will include the desktop environment in the ISO name. For example, *"artix-(desktop-environment)-openrc-(date-of-release)-x86_64.iso"*. Once you have gotten your ISO, fire up balenaetcher and select your ISO. Then proceed to select the USB device and click "Flash!". **This will whipe everything on your USB so make sure to back everything up!**

*Links:*
Artix Linux ISO Download: https://iso.artixlinux.org/isos.php
balenaetcher Download (Windows, MacOS, Linux download): https://www.balena.io/etcher/

## Boot from the USB or CD Drive
Afterwards, you can reboot into your Boot Menu. Your Boot Menu button is different for every motherboard. For me, the Boot Menu button is F11. So I would reboot and press F11 to get to my boot menu, and then I would select the USB you just made.
