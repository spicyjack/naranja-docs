See also the Linux Encryption page in the Antlinux Wiki.

Change the key password

gpg --no-default-keyring --secret-keyring ./keystore.sec  --keyring ./keystore.pub --edit-key
Command> passwd
This key is not protected.
Enter the new passphrase for this secret key.
Command> quit
Save changes? (y/N) y
List your keys

#!/bin/sh

# script to list all of the keys in the secret keyring

/usr/bin/gpg --no-default-keyring \
    --secret-keyring ./keystore.sec \
    --keyring ./keystore.pub --list-secret-keys
Determine the key ID

/usr/bin/gpg --no-default-keyring    --secret-keyring ./keystore.sec    --keyring ./keystore.pub --list-secret-keys | grep "^sec" \
| sed 's/^sec .*1024D\/\(.*\) .*/\1/'
Determine key ID without the newline (for scripting)

/usr/bin/gpg --homedir /mnt/flash/keys/ --list-secret-keys --lock-never \
--trust-model=always 2>/dev/null | grep "^sec" \
| sed 's/^sec .*1024D\/\(.*\) .*$/\1/' | tr -d '\n'
Determine which public keys were used to encrypt a disk key file

gpg --batch --decrypt --list-only --status-fd 1 2>/dev/null | awk '/^\[GNUPG:\] ENC_TO / { print $3 }'
Generating many GNUPG private/public keys at once

http://cvs.antlinux.com/cvsweb.cgi/antlinux/scripts/keystore/ batch.gnupg:
%echo Generating a standard key
Key-Type: DSA
Key-Length: 1024
Subkey-Type: ELG-E
Subkey-Length: 1024
Name-Real: Disk Encryption Key
Name-Comment: key has no passphrase
Name-Email: dns at antlinux dot com
Expire-Date: 0
#Passphrase: abc
%pubring keystore.pub
%secring keystore.sec
# Do a commit here, so that we can later print "done" :-)
%commit
%echo done
gnupg-batch.sh:
#!/bin/sh

# run gnupg multiple times generating keys
while /bin/true; do
    time /usr/bin/gpg --batch --gen-key batch.gnupg
    sleep 5m
done
Average key generation time on a light-medium loaded server is about 5 minutes once the entropy pool is run through.

Creating Keys for loop-aes

Creating the 65 keys for use with loop-aes encryption:

#!/bin/sh

head -c 2925 /dev/random | uuencode -m - | head -n 66 | tail -n 65 \
| gpg --symmetric -a > output_file.gpg
The test passphrase is passphrase.

Add this to your /etc/fstab file to mount the encrypted partition:

/dev/hda666 /mnt666 ext2 defaults,noauto,loop=/dev/loop3,encryption=AES128,gpgkey=/a/usbstick/keyfile.gpg 0 0
The "losetup -F" command asks for passphrase to unlock your key file. Losetup -F option reads loop related options from /etc/fstab. Partition name /dev/hda666, encryption=AES128 and gpgkey=/a/usbstick/keyfile.gpg come from /etc/fstab.

    losetup -F /dev/loop3
    mkfs -t ext2 /dev/loop3
    losetup -d /dev/loop3
    mkdir /mnt666
Now you should be able to mount the file system like this. The "mount" command asks for passphrase to unlock your key file.

    
    mount /mnt666
Check that loop is really in multi-key-v3 mode. losetup -a output should include string "multi-key-v3" indicating that loop is really in multi-key-v3 mode. If no "multi-key-v3" string shows up, you somehow managed to mess up gpg key file generation part or you are trying to use old losetup/mount programs that only understand single-key or multi-key-v2 modes.

    losetup -a
Got a question? Get answers at the project-naranja Google Groups page.
