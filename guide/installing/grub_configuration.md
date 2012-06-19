Here's how to install GRUB on different types of bootable devices...
As a shortcut when using any of the GRUB commands that require a hard drive specification, you can hit the <TAB> key, and GRUB at that point will print out a list of possible devices that are applicable to the command you are trying to use.

Flash Drive

In order to use a USB flash drive (also known as a thumbnail drive or key drive) as a GRUB boot device, your motherboard BIOS must support booting from these kinds of devices. If it does, perform the following steps to get GRUB to boot from this device.

Copy the GRUB files from either /boot/grub or /lib/grub/<arch> where <arch> is the processor type you wish to use GRUB on.
Run the GRUB shell by typing grub at the command prompt. Most likely, you'll need to be root to do this, as GRUB will want to write to the device when it installs, and most disk 'block device' files allow only the root user write access.
Figure out which device you wish to install GRUB to. GRUB doesn't use the standard Linux convention for naming it's devices, it does it's own thing (it goes over the devices that the BIOS of the machine says are available). To do this right, you have to figure out which devices are which in GRUB; the geometry command can help here, use it to get GRUB to display the total size of the device. Also try the <TAB> hint explained above.
grub> geometry (hd0)
drive 0x80: C/H/S = 25665/255/63, The number of sectors = 1465149168, /dev/sda
  Partition num: 0,  Filesystem type is ext2fs, partition type 0x83
  Partition num: 1,  Filesystem type unknown, partition type 0x8e

grub> geometry (hd1)
drive 0x81: C/H/S = 15/255/63, The number of sectors = 252928, /dev/sdb
  Partition num: 0,  Filesystem type is ext2fs, partition type 0x83
Tell GRUB what the root partition is, i.e. where the grub files are installed. You do this with the root command.
grub> root (hd1,0)
Filesystem type is ext2fs, partition type 0x83
Run the setup command with the GRUB device you wish to install GRUB to; you can use the GRUB name for the whole device (example setup (hd0)) or you can install GRUB onto a partition of that device (example: setup (hd0,0)). You would install GRUB to a partition instead of to the whole device if you were going to load GRUB from another boot loader.
Exit GRUB with the quit command once you're done editing the partition table.
Make that partition bootable with the set command in parted. The below command makes the first partition on the current disk bootable.
parted /dev/sda
set 1 boot on
quit
Got a question? Get answers at the project-naranja Google Groups page.
