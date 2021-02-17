# Easy Artix Installation

What is Artix Linux? Artix Linux is a Linux distubution that is based off the popular distrubtion Arch Linux. So what differs from Artix and Arch Linux? Arch uses the popular init system called "SystemD". SystemD isn't necessarily bad, the reason why most people want to use another init system besides SystemD is because of the "non-bloat" potential of the init system. SystemD doesn't have isolated containters/groups which each program can run in, so it doesn't follow the "UNIX Philosophy". There are other init systems such as OpenRC, s6, and runit.

Artix has multiple ways to install. You get to install it via a GUI like Manajaro or Ubuntu, while there is another way such as using a CLI (Command Line Interface). Today I will be installing it using the terminal for the minimal install so we can get as least bloat as possible.
