# PPCLinuxDocumentation
 Documentation for Linux on PowerMacs (big-endian PPC).

## 1. Distribution Support
- [Ubuntu](http://ubuntu.com) 16.04 (xenial) and earlier (No longer updated)
    - Not recommended, since the "xenial-updates" repository seems to be down (likely archived somewhere, but I can't find it)
    - Only [Ubuntu MATE](https://releases.ubuntu-mate.org/archived/xenial/) and Lubuntu (no longer available from official server) were available past around 14.04
    - For more information on support, see https://wiki.ubuntu.com/PowerPCKnownIssues (useful resource for all distros listed). 
- [Debian](http://debian.org) 8 and earlier (No longer updated)
- Debian unstable (sid)
    - Hidden on the Debian website, can be found at https://cdimage.debian.org/cdimage/ports/current/
    - See https://forums.macrumors.com/threads/the-powerpc-debian-wiki.2178457/ for additional information (used as a source throughout but appears slightly outdated)
    - When asked for a root password, press Enter twice in order to use sudo instead. Sudo is used in the remainder of the guide.
- [MintPPC](https://www.u58733p55594.web0093.zxcs-klant.nl/) (Debian PPC with a slightly easier installation process)
- [Void Linux PPC](http://voidlinux-ppc.org)
    - Not recommended, dropping Apple Mac support **entirely** in early 2023
    - If used, the bootstrap (Apple_Bootstrap) partition **must be 256MB or greater.** If it's not, the installation **will hang indefinitely.**
    - The GUI will not launch on the NVIDIA GeForce FXGo5200 with the default kernel (and maybe other NVIDIA cards). Install with the fallback kernel, then run "xbps-install -Su" on the installed system to update it (booted through the fallback kernel).
    - [ArchPOWER](http://archlinuxpower.org)
    - I've never had any success personally getting it to work, but that could be because I'm an Arch Linux noob.
- [Gentoo](http://gentoo.org)
    - May have issues compiling (untested)
- [Adelie Linux](http://adelielinux.org)
    - Slow development, the current ISO (as of 7/18/2022) won't install without [workarounds](https://youtu.be/AArGaJGFVH4).
    - Uses the musl C library instead of glibc (making the term "GNU/Linux" factually incorrect!), so existing software will have to be recompiled (although the distro seems to be notably faster than any glibc distro)
## 2. Bootloaders
- [Yaboot](https://github.com/yaboot/yaboot)
    - Used on Ubuntu and Debian 8 and earlier
    - Not updated frequently but still functional and can boot modern kernels
    - Can chainload MacOS
- GRUB
    - Slow
    - Cannot chainload MacOS
    - Still updated
    - Used on ArchPOWER, Gentoo, and Debian
    - Doesn't work on G3 machines with low RAM
## 3. Graphics card support
### a. NVIDIA cards
- The NVIDIA GeForce2 MX will not load ANY GUI since 2011. This is due to a long-standing bug in the nouveau driver. 
    - All workarounds found online tell you to use the old "nv" driver (currently used on OpenBSD for PowerPC Macs) but this hasn't worked since Ubuntu 14.04
- Some supported cards don't seem to play hardware-accelerated video back properly (endianness issue)
### b. ATI cards
- Some cards made before 2003 will not load ANY GUI in my experience. However, this isn't documented anywhere else, so it might just be my specific hardware.
- On older kernels (such as those found in Debian 8 and Ubuntu 10.04-16.04) the code "radeon.agpmode=-1" (forcing those cards to run in PCI mode) had to be added to the kernel parameters to prevent freezing. As of kernel 4.19 this is no longer required. 
- The cards appear to the system as "AMD ATI" cards (purely a cosmetic bug)
- The "radeon" driver is far more stable than the NVIDIA driver on big-endian (but not as stable as the Mac OS X driver)

## 4. WiFi
- Drivers are available for all of the 2 WiFi cards used on PowerPC Linux, but you will need an ethernet connection.
- On Ubuntu, the drivers are available by typing "sudo apt-get update && sudo apt-get install firmware-b43-installer" for 2003-and-later (mini-PCI & all non-removable) cards, or "sudo apt update && sudo apt install firmware-b43legacy-installer" for 1998-2002 (PCMCIA) cards. 
- Follow this [guide](https://docs.voidlinux-ppc.org/configuration/apple.html#wireless-networking) for WiFi on Void Linux.
- Follow the "Other distributions" section of [this guide](http://linuxwireless.sipsolutions.net/en/users/Drivers/b43/#Other_distributions_not_mentioned_above). Before the guide is followed, run "sudo apt install build-essential module-assistant" in the command line. Note that this solution is not perfect: internet seems to be very slow in this case.
## 5. CRTs
Most CRT Macs require a custom xorg.conf. If you manage to get a GUI working, please let me know by submitting a pull request or issue. If you have an eMac with a Radeon 7500, check out https://github.com/75mhzMac/eMac7500.
## 6. What doesn't work
- Touchpads: While they generally work as generic pointing devices, they tend to not respond correctly or "jitter" when you keep your mouse in the same spot.
- Sleep/Wake: While there are workarounds to getting it to work, they usually require backporting older packages from older releases and are therefore not recommended.
- Some older graphics cards, as detailed [here](https://docs.voidlinux-ppc.org/configuration/graphics.html).
- Battery indicators: While they technically work if you load the "pmu_battery" module (sudo modprobe pmu_battery), they tend to be somewhat inaccurate and take up to 30 seconds to detect a charger connection or removal.
- Modern versions of the Qt5 library on glibc based distros (Debian, Ubuntu, Void) do not allow you to use the mouse to navigate certain menus. This is probably an endianness bug. As a workaround, you can use the arrow keys, Enter, and Esc to select items when the mouse won't work. GTK2 and GTK3 do not have these issues, but perform a bit worse than Qt5.
- Sound: While workarounds exist to get sound working, many are broken in modern kernels and **are unsafe** on certain computers due to the speakers not being calibrated properly. **If you decide to enable sound in some way (or use an older distro that supports it, like Ubuntu 6.06) you may run the risk of blowing out your speaker. [Don't ask me how I know.](https://youtu.be/ph1LXMO1m2o) USE AT YOUR OWN RISK!**

All other features, including FireWire, USB, Ethernet, CD/hard drives, and Bluetooth work.
