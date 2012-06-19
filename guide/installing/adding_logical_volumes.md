Table in EVMS User's guide listing which filesystems can be expanded/shrunk and whether or not the resizing operation needs to be done with the filesystem mounted or unmounted.

See the LVMEncryption Wiki page for the specific steps, which should eventually show up here as well. Transcript of Volume creation:
# mount the loopback partition
~ # /sbin/losetup -G /mnt/flash/keys/ -K /mnt/flash/keys/user_disk_key.gpg \
> -e AES128 /dev/loop0 /dev/sda2
Password:

# another version that can be scripted
# the 'p' means "read the passphrase from this file descriptor", with '0'
# being STDIN
echo "somelongasspassphrase" | /sbin/losetup -G /mnt/flash/keys/ \
> -K /mnt/flash/keys/user_disk_key.gpg -p 0 \
> -e AES128 /dev/loop0 /dev/sda2

~ # pvcreate /dev/loop0
  Physical volume "/dev/loop0" successfully created
~ # vgcreate evg0 /dev/loop0
  Volume group "evg0" successfully created
~ # vgchange -a y evg0
  0 logical volume(s) in volume group "evg0" now active
~ # lvcreate -L1000 -nrootvol evg0
  Logical volume "rootvol" created
~ # lvcreate -L3000 -nvarvol evg0
  Logical volume "varvol" created
~ # lvcreate -L7000 -nusrvol evg0
  Logical volume "usrvol" created
# see how much free space is left
~ # vgdisplay evg0 | grep Free
  Free  PE / Size      176101 / 687.89 GB
# NOTE: the below command uses a lowercase 'l' instead of the uppercase 'L'
# lowercase 'l' == extents, uppercase 'L' == size in Mb
~ # lvcreate -l175600 -nhomevol evg0
  Logical volume "homevol" created
# now create the filesystems
~ # mkfs.xfs /dev/evg0/rootvol
~ # mkfs.xfs /dev/evg0/varvol
~ # mkfs.xfs /dev/evg0/usrvol
~ # mkfs.xfs /dev/evg0/homevol
# this last partition is the CF IDE hard drive, which will end up being /boot
~ # mkfs.ext2 /dev/sdb1
~ # mount -t xfs /dev/evg0/rootvol /mnt/rootvol
~ # mkdir /mnt/rootvol/boot
~ # mount -t ext2 /dev/sdb1 /mnt/rootvol/boot/
~ # mkdir /mnt/rootvol/usr
~ # mount -t xfs /dev/evg0/usrvol /mnt/rootvol/usr/
~ # mkdir /mnt/rootvol/var
~ # mount -t xfs /dev/evg0/varvol /mnt/rootvol/var/
~ # mkdir /mnt/rootvol/home
~ # mount -t xfs /dev/evg0/homevol /mnt/rootvol/home
