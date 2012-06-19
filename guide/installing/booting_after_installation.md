After the base system has been installed, you boot the system using the supplied kernel and initramfs image. Once the Linux kernel boots and the init script in the initramfs image runs, the script will start a web server on a pre-determined port in order to serve HTTP requests. The web server has been wrapped with the stunnel SSL tunnelling program for lots of crunchy SSL goodness in order to protect your disk keys.

Disk Key Naming Conventions
The disk key should be named disk_key.gpg, and the GPG keys should be named <KEYIDXXX>.[pub|sec], where KEYIDXXX is the 8 character 'key fingerprint' of the GPG key, and .[pub|sec] is the filename extension for the public and secure keyring respectively. For now, the keys also need to be copied as secring.gpg and pubring.gpg for the private and public keyring respectively. This will change in the future to allow prompting for random keys. The scripts in the initramfs image will follow these conventions when they go to look for GPG or disk keys.

Example file names:

disk_key.gpg
FFFE1F22.pub
FFFE1F22.sec
secring.gpg
pubring.gpg

Bringing the System Up
There are three distinct ways to bring the system up once the kernel/initramfs image had been loaded:

Insert a USB thumbdrive with the disk key and user GPG keys located in a directory called /keys. Once the system sees the USB thumbdrive, the drive is mounted read-only and the GPG/disk keys are searched for in the /keys directory. This option is only available via the system console, meaning you need a monitor and keyboard connected to the machine and need to be sitting in front of it in order to insert the USB thumbdrive and type in the passphrase to unlock the disk key. The GPG/disk keys must follow the naming conventions above in order to be found by the setup scripts.
Upload the user keys and disk key via HTTP or HTTPS (preferred) by using a CGI script. The boot scripts will have the machine either come up on a static IP or use DHCP to obtain an IP address, and then start thttpd/stunnel in order to allow admins to access the CGI script that is used unlock the system. The disk keys are stored in RAM, and only remain on the system long enough to bring the system online. In this case, you can name the keyfiles anything you want, but you must upload the correct keyfiles using the upload boxes on the CGI form in order to boot the system
A hybrid of these two methods is also available, where you insert a USB thumbdrive containing the disk keys and user GPG keys (located in /keys), and then use the web interface to unlock the disk and bring the system up to a running state. You give the CGI script the key ID of the key you wish to use to unlock the system, as well as the passphrase needed to use that key, and then submit the information to the server to run the CGI script and boot the system
