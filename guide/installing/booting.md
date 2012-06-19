Boot the system either with a Project Naranja CD-ROM image, or by booting the machine via PXE. Here's a page from the Antlinux Wiki on how to set up PXE boot. A bootable CD with the Project Naranja kernel/initramfs image can be created using something like ISOLINUX or GNU GRUB.

When booting the system, you can add some keywords to the Linux kernel command line that will make the initramfs image perform different actions based on which keyword you use.

run: this keyword tells the init script to run some other program instead of letting itself run completely. Running the init script from start to finish should bring the system up to a normal running state. The run command has the following options:
run=sh: halts normal system booting, and starts a simple shell. There is no login prompt or access control whatsoever. The shell that runs on the initramfs image is ash, a shell descended from Documentation on the 'dash' shell, which was generated from the manpage, is available in the appendix.
run=init: runs the Busybox version of init. init on Unix machines is the 'master process', it bootstraps the system and starts most (if not all) of the server applications that run on the system. If init goes away, the machine will most likely crash. The inittab that is used is available so you can see what is run; note that a shell is run on the serial device /dev/ttyS0, so that you can connect to the machine and run commands (including performing a full install) using a null-modem cable from another machine. The serial port settings are 9600 baud, 8N1 bits/parity/stop bits.
run=install: runs the Project Naranja Installer, which is still in development. Once it's complete, it will be a curses-based installer that can be used to build a Project Naranja system from scratch.
The following keywords are used to tell the initramfs image where to find specific hardware devices, or gives it hints on how to set up the system once the initramfs image is loaded.

rootdev ($ROOT_DEV) (required, no default) - The device (block device file, which usually corresponds to a partition on a hard drive) that contains the encrypted volume. Once the system comes up, the system will try to use the disk key and passphrase to unlock the encrypted patition on this device; this parameter tells the system which partition to attempt to unlock.
volgrp ($VOLUME) (required, no default) - Sets the name of the LVM2 volume group to bring online once the encrypted partition has been mounted. You need this if you are using LVM2 on top of an encrypted partition in order to manage disk space. If the init script does not find a vol parameter, it will stop script execution and ask if a debug shell should be run in order to troubleshoot any problems.
rootvol ($ROOT_VOL) (required, no default) - The name of the root volume to mount and run /sbin/init from. The full path to the root volume is made up using the name of the root volume combined with the name of the volume group; for example, if the root volume was named rootvol and the volume group was named vg0, then the system would try to mount /dev/vg0/rootvol and execute the /sbin/init program in order to bring the system up to a running state. If the init script does not find a root parameter, it will stop script execution and ask if a debug shell should be run in order to troubleshoot any problems.
thumbdev ($THUMB_DEV) (optional, defaults to /dev/sdc1) - Device to watch for USB thumbdrive insertion. Once a USB thumbdrive is inserted into the machine and comes up as this device, the thumbdrive will be mounted and a directory called /keys will be searched for, in order to read in the keys to unlock the system from the thumbdrive.
prompt ($PROMPT) (optional, no default) - If prompt=no is set on the boot command line, the prompt.sh script will not be run. You would only use this option if you wanted to boot a system that does not use encryption at all; you would not be able to bring up an encrypted system if you use prompt=no. You have been warned!
aeskey ($AES_KEY) (optional, default is 128 bit AES keys) - Sets the keylength on the block device. Default is 128 bit keys (16 byte key), but the VIA PadLock chipsets also support keylengths of 192 and 256 bits (24 and 32 byte lengths).
Hint: the file_parse() shell script function is what parses the commands given to the kernel at boot time; this function is located in the ant_functions.sh shell script, which is sourced by the initramfs init script when it is started by the kernel after the initramfs image has been loaded from storage into memory and un-archived.

Sample PXELINUX config:

SERIAL 0 9600
TIMEOUT 300
PROMPT 1
DISPLAY /HOSTNAME/banner.txt
DEFAULT naranja-init
LABEL naranja-init
        KERNEL /HOSTNAME/vmlinuz-2.6.XX.X-naranja
        APPEND console=ttyS0,9600n8 console=tty0 initrd=/HOSTNAME/initramfs-2.6.XX.X-naranja.cpio.gz vol=vg0 root=rootvol rootdev=/dev/sda2  thumbdev=/dev/sdb1 run=init
LABEL naranja-boot
        KERNEL /HOSTNAME/vmlinuz-2.6.XX.X-naranja
        APPEND console=ttyS0,9600n8 console=tty0 initrd=/HOSTNAME/initramfs-2.6.XX.X-naranja.cpio.gz vol=vg0 root=rootvol rootdev=/dev/sda2  thumbdev=/dev/sdb1
The above configuration would be placed in the HOSTNAME directory located inside of the $TFTP_ROOT directory, along with the kernel image and initramfs image and any other needed support files ('banner.txt' file for example). The APPEND lines above should be all one line instead of wrapped to the width of the console as shown above. You can get more help with setting up PXE booting on the How to set up PXE boot page of the Antlinux Wiki. The PXELINUX page may also have more helpful information on getting PXELINUX up and running.

Got a question? Get answers at the project-naranja Google Groups page.
