To get help with Project Naranja, join the Project Naranja Google group and post your questions there. Here are links to all of the external web pages mentioned in the previous project pages:

Project Naranja Downloads

Project Naranja Google Code page
Project Naranja kernel and initramfs image downloads (vmlinuz-*, initramfs-*.gz), and Debian packages for the Linux kernel (linux-image-*.deb) and loop-aes encryption software (loop-aes-*.deb, loop-aes-ciphers-*.deb).
There is also a PDF copy of this documentation available on the Project Naranja Google Code downloads page as well.
Support Pages

Google Groups
Project Naranja group
Tom Dunigan's site has a very complete list of cryptography hardware vendors
Linux-crypto mailing list
Other Encrypted Disk Projects

Software Suspend/Loop-AES/Smartcard Disk Encryption
Vinvin's VIA C7 loop-aes page
The below links list the various resources that went into creating this project.

Hardware Vendors

VIA Technologies, Inc.
VIA EPIA mini-ITX motherboards
Padlock Security Engine
"Secure by Design" lists the different features of the VIA processors as far as how well they do cryptography operations
EN Series Motherboards
SP Series Motherboards
VIA SP13000 motherboard
Soekris Engineering
PCI and Mini-PCI crypto boards based on the Hifn chipsets.
Hifn
SafeNet
Software

Linux
Debian GNU/Linux
Busybox, the swiss-army knife of embedded tools
Michal Ludvig's VIA Padlock support in the Linux kernel site
His article in Linux Journal on the VIA Padlock Encryption engine
rng-tools package, extensively patched by a Debian developer, speeds up GPG key generation by a significant amount, because it helps keep the Linux kernel's entropy pool nice and full
diceparse.pl Perl script from Antlinux CVS
Diceware home page, where you can obtain lists of Diceware passwords in different languages and in different formats (text, PDF, etc.)
LVM2, the second generation of Logical Volume Management software for Linux
loop-aes Linux kernel encryption module for storage devices
bonnie++, a disk and filesystem benchmarking tool
Interpreting bonnie++ results
GNU Parted, a disk label/partition map/partition table editing tool (all those terms mean the same thing).
mklabel command
mkpart command
print command
GNU GRUB
GRUB Manual
Installing GRUB natively
Installing GRUB using grub-install
GNU Privacy Guard (GPG)
GPG Documentation index
