How it will work...

The base system will be bootable from either a CD-ROM drive or from the
network using the PXE-capable network card built into the newer models of the
EPIA motherboards. Once the base encrypted filesystems are up and running, a
simple install of Linux can be performed in order to make the system
functional. Currently the base system can perform an install of Debian
GNU/Linux, but it would be nice to see more flavors of Linux working. After
reboots, a SSL-enabled web server and console application (on the TTY/serial
port) runs from the initramfs image to allow remote-administered mounting of
filesystem in order to bring the system up to provide services to local and
remote users.

Terminology (borrowed from the loop-aes README)

key file
The key file is a set of random strings of data. When the encrypted device is
mounted, the key file is used to read/write encrypted data to and from the
encrypted block device. Normally, the key file is protected using encryption
of some kind. In version 1 of loop-aes, the key file consists of one string of
random data. In versions 2 and 3 of loop-aes, the key file consists of 64 and
65 strings of random data respectively. Each time a block of data is written
to the encrypted block device, the next random string in the key file is used
to encrypt that data, with this pattern repeating from the beginning when all
of the random strings of data have been used to encrypt data written to the
block device. A key file looks like this:

    8bYXCh6PfdWfbqpjFa5FspLb7TIlK1cPnWNvn1PxHZYMjPlquGmNg71I1/QR
    fjl2II0XBezltUsEUM1cYHqhtPb6tWoUoYNs6a+LdnK03JMp/xxH5VgAXnb2
    ...[more lines of text]...

GPG encrypted key file
One (or more) 'key files' (above) are protected by using GNU Privacy Guard
(hereafter called 'GPG') to encrypt/armor the 'key file'. A 'key file'
protected with GPG looks something like this:

    -----BEGIN PGP MESSAGE-----
    Version: GnuPG v1.4.1 (GNU/Linux)

    W58dYnHwjlAhE2o2umhq5o0AdTEeFDeDz0BGBKwKPavBhh4tK0bFCnuWIQmV
    m1t7RTU8UDvmkhFajQtTAECOjpMQJZNwjNznso3l5bbapEXwNxMF2H+IUMkB
    r41C+Hnlvgyx/i8ti+7HOC7XakzH9SP6FlcrzFIs+53TFURw0jy6SHHJcm8T
    ...[more lines of text]...

symmetrical encryption
Protecting the 'key file' with GPG by encrypting it with a simple password
asymmetrical encryption
Using one or more GPG public keys to protect the 'key file'. You could gather
a list of public keys from users that you trust, and add all of their keys to
the 'key file', thereby allowing those trusted users to unlock the encrypted
partition.
System Setup Details

When you use asymmetrical encryption, you can use the public keys of multiple
trusted users, and then those users would be able to mount/unmount the
encrypted block device as needed/desired. Another way to use asymmetrical
encryption would be to create mulitple GPG public/private key pairs for the
sole purpose of encrypting a 'key file', then using one GPG key at a time to
mount the encrypted partition and bring the system to a running state; after
the system is up and running, the GPG key pair that was used to bring the
system up is discarded and the GNG public key is removed from the 'key file'.
By using multiple GPG keys, the system admin can hand out GPG keys to trusted
users in order to bring systems online if the primary admin is not available.
The users don't have to manage the 'key file' with their personal GPG keys (if
they even have their own personal GPG keys that is).

Once the encrypted storage device is available via loop-aes, Logical Volume
Management is used to create and manage partitions on top of it.


We need keys. Lots of keys.
Development and Testing

The development and test environment consists of two VIA EPIA SP13000
motherboards mounted in custom cases. Work has already started in testing and
benchmarking the performance of the Padlock Security Engine for use in
encrypting whole partitions. See the benchmark testing overview page for lists
of benchmarks taken with and without Padlock encryption via the loop-aes Linux
kernel module.

Misc. Information

The Project Naranja wiki page has some ideas of things that the project will
incorporate.

See also the Naranja kernel building page in the wiki for more information on
what options I am including in my Naranja kernel.

Got a question? Get answers at the project-naranja Google Groups page.
