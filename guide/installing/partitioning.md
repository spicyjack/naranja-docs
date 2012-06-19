FIXME The Wiki Page on Parted has some better ideas about how to explain this
next section, i.e. you can do a bunch of partitions or none at all depending
on how you set things up.

Here's an example of using the partitioning tools to create partitions using
the tools provided in the naranja initramfs image. There's also much more
information on using parted in the Antlinux Wiki Parted Page.

~ # parted
GNU Parted 1.6.21 with HFS shrink patch 16
Copyright (C) 1998 - 2004 Free Software Foundation, Inc.
This program is free software, covered by the GNU General Public License.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more
details.

Using /dev/sda
The mklabel command creates the MS-DOS partition labels that most operating
systems that run on Intel machines can understand. Other partition types
besides msdos are listed in the GNU parted manual page for mklabel (which is
linked from the Project Naranja links page).
(parted) mklabel
New disk label type? msdos
Next, create a partition to hold the contents of the /boot directory. This
partition cannot be encrypted, as the bootloaders you use need to be able to
read it to pick up the initramfs image that contains all of the tools you will
need to actually mount the encrypted partitions. The below mkpart command
creates a primary MS-DOS partition that starts at byte 0 and that is 100
megabytes in size, and formatted with the ext2 filesystem:

(parted) mkpart primary ext2 0 100
Create the encrypted partition with the rest of the available space. You can
see the total size of the drive in megabytes by using the print command;

(parted) print
Disk geometry for /dev/sda: 0.000-715404.867 megabytes
Disk label type: msdos
Minor    Start      End    Type      Filesystem  Flags
1          0.031    101.975  primary
(parted) mkpart primary 101 715410
(parted) print
Disk geometry for /dev/sda: 0.000-715404.867 megabytes
Disk label type: msdos
Minor    Start      End    Type      Filesystem  Flags
1          0.031    101.975  primary
2        101.975 715402.375  primary
(parted)              
Also note that there's only two partitions on the drive. "Well, what about all
the other filesystems and swap partition? Don't you need those too?" The drive
can be partitioned further once LVM2 is up and running on the encrypted
partition.

~ $ parted /dev/sda
(parted) mkpart primary 0 2000
(parted) p                                                                
Disk geometry for /dev/sda: 0.000-715404.867 megabytes
Disk label type: msdos
Minor    Start      End    Type      Filesystem  Flags
1          0.031  2000.280  primary  ext2        
(parted) mkpart primary 2000 715404                                      
(parted) mkfs 1 linux-swap
(parted) p                                                                
Disk geometry for /dev/sda: 0.000-715404.867 megabytes
Disk label type: msdos
Minor    Start      End    Type      Filesystem  Flags
1          0.031  2000.280  primary  linux-swap  
2      2000.281 715402.375  primary              
(parted) q
~ $ parted /dev/sdb
(parted) mkpartfs primary ext2 0 486                                      
(parted) p                                                                
Disk geometry for /dev/sdb: 0.000-487.757 megabytes
Disk label type: msdos
Minor    Start      End    Type      Filesystem  Flags
1          0.031    486.342  primary  ext2        
(parted) set 1 boot on                                                    
(parted) p                                                                
Disk geometry for /dev/sdb: 0.000-487.757 megabytes
Disk label type: msdos
Minor    Start      End    Type      Filesystem  Flags
1          0.031    486.342  primary  ext2        boot
(parted) q                                                                
Got a question? Get answers at the project-naranja Google Groups page.
