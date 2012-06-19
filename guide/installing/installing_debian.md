See also the wiki page on Debootstrap. The Debian 'stable' release install guide has a whole section dedicated to using debootstrap.

For some reason, the DHCP client included with busybox didn't want to work for me. The boot image uses dhclient, from ISC's DHCP package. I also included instructions on setting up a static IP below just in case.

# get the network going using DHCP
/sbin/dhclient eth0 # replace 'eth0' with the correct interface on your machine

# if DHCP fails, do it static-like
# replace the below IP's with the appropriate info for *your* network
ifconfig eth0 192.168.1.2 netmask 255.255.255.0 broadcast 192.168.1.255 up
# add a route for the default gateway
route add default gw 192.168.1.1
# add a nameserver
echo "nameserver 192.168.1.1" > /etc/resolv.conf

# then run debootstrap
debootstrap --arch i386 sarge /mnt/rootvol http://ftp.us.debian.org/debian
Since the current releases of Project Naranja are built from Debian Etch, the following debootstrap targets are available in the Etch release of debootstrap as of Project Naranja release 2007.dos.1. The deboostrap targets below are listed in order from newest (top of each list) to oldest (bottom of each list):

Debian
Sid (Currently codenamed Lenny)
Etch
Sarge
Woody
Potato
Ubuntu
Breezy Badger (breezy, version 5.10)
Hoary Hedgehog (hoary, version 5.04)
Warty Warthog (warty, version 4.10)
You can view the debootstrap(8) manpage for more information about extra debootstrap options and release targets.

Once you get the base system installed, you have a few more things to do before you can reboot the machine and start running the machine in production.

You can use some of the files on the initramfs image (/etc/fstab, /etc/network/interfaces, /etc/resolv.conf) on your new system by copying them into /mnt/rootvol prior to running chroot below. Make sure you change the paths in /etc/fstab to match how the hard drives/mount points are configured on your system prior to chroot'ing/rebooting.

Edit the fstab file and add your LVM partitions that you set up previously. Make sure you add the paths as they will exist once the system has been rebooted and running by itself, not how it exists inside of the initramfs image. There's an example /etc/fstab in the Debian documentation linked from the the wiki page on Debootstrap (hint: your busybox binary probably has an editor or two that you can use if the base system lacks one)
chroot into the new filesystem so that you can edit some of the config files and run the base-install program
$ LANG= chroot /mnt/rootvol /bin/bash
Add the Debian package signing keys to the system.
apt-get install debian-archive-keyring
apt-get update
Mount the filesystems either by hand (mount -t [fs-type] [block device] [mount point]) or with mount -a
Mount the /proc partition
# this should work
$ mount -t proc proc /proc

# verify that you see files in /proc
$ ls /proc

# if not, exit the chroot with either 'exit' or Ctrl-D, and try this:
$ mount -t proc proc /mnt/debinst/proc
Set the root password
passwd
Configure your keyboard
$ apt-get install console-data
$ dpkg-reconfigure console-data
Configure the network interfaces by editing /etc/network/interfaces. There's an example file listed in the Debian documentation linked from the the wiki page on Debootstrap
Create a simple /etc/resolv.conf file so you can resolve servers using DNS
Set up things like timezones, users, apt sources with /usr/sbin/base-config new
If you want to set up locales, install and configure the packages for locales
$ apt-get install locales
$ dpkg-reconfigure locales
Set up a kernel on the new system
If you are using LVM2 on your system, install the lvm2 and lvm-common packages via apt-get; when the init script runs switch_root, the kernel will try to re-create the LVM block devices in /dev, which it can't do without the LVM tools
You also need to install the filesystem tools for whatever type of filesystem you are using on your encrypted partitions; when the kernel runs /sbin/init, it reads /etc/fstab in order to mount all of the partitions listed in that file; prior to mounting the partition, it goes to look for the fsck tool assosciated with the filesystem on that partition, and will refuse to mount that partition if it can't find the correct fsck tool.
Some other packages to consider installing prior to rebooting:
ssh
loop-aes-utils
Project Naranja kernel and loop-aes Debian packages from Sourceforge
module-init-tools (for 2.6 kernels; if you're using the Project Naranja kernel/initramfs, you'll want this package installed)
Set up the boot loader of choice to boot the kernel and the initramfs image that has the correct scripting/kernel modules so that when the system is rebooted, you'll be prompted to mount the encrypted partition so the system can boot. The instructions for setting up GNU GRUB are on the next page.
If you miss any of the steps above, you can always boot to the setup program as if you were going to run debootstrap again, and mount all the partitions in order to chroot into the installed system and install missing packages or run commands on the installed system.
