You'll need at least one 'key file' for encrypting the data that gets written to the disk. This key can be protected via GNU Privacy Guard (GPG) using either a simple password (symmetrical encryption), or with one or more public keys (asymmetrical encryption).

See the terminology section of the Introduction page for a better explaination of encryption keys, examples of how they would be used, and definitions of the terms used in this document. The encryption key setup notes page has more details on how to generate a 'key file' and protect it with symmetric (simple password) encryption. Instructions for using public keys (asymmetrical encryption) are below.

The below steps assume you have already started rngd on top of the EPIA motherboards with Padlock encryption; if you are not running rngd or not using a Padlock-enable motherboard, it will take much longer to generate GPG keys. See the EPIA Encryption benchmark page for the hard numbers.

Set up your GPG keys. You'll need your GPG keys in $GNUPGHOME for any of the next steps to work. The below steps also assume that you're not using your regular GPG keys, hence the usage of $GNUPGHOME as well as the GPG switches --no-default-keyring --always-trust --lock-never. There's a recipe for batch generation of GPG keys, it's available on the managing encryption keys page. If you need a quick GPG key, you can create one with the following command, assuming you are replacing $GNUPGHOME with the directory you want GPG to use for your public/private keypair.
/usr/bin/gpg --homedir $GNUPGHOME --gen-key
Import all of the other public keys you wish to use with this 'key file' into the master key.
gpg --no-default-keyring --homedir $GNUPGHOME --always-trust \
--lock-never --import [KEY1.pub KEY2.pub ... KEY20.pub]
# or try it this way:
gpg --no-default-keyring --homedir $GNUPGHOME --always-trust --lock-never --import $(ls *.pub)
If you are just going to use one GPG key with the 'key file', you can create the 'key file', and then protect it with the your GPG key
head -c 2925 /dev/random | uuencode -m - | head -n 66 | tail -n 65 \
| gpg --no-default-keyring --homedir $GNUPGHOME --armor --encrypt \
--default-recipient-self --recipient 0xXXXXXXXX \
--always-trust --lock-never \
> master_disk_key-XXXXXXXX.gpg
If you imported multiple GPG public keys to a GPG keyring, re-encrypt the 'key file' so that the freshly-imported public keys can now unlock the disk key
# generate a list of keys in the public keyring for reference
gpg --no-default-keyring --homedir . --fin | grep pub
# now decrypt then re-encrypt the existing disk key
gpg --no-default-keyring --homedir . --decrypt < master_disk_key.gpg \
| gpg --encrypt --armor --homedir $GNUPGHOME \
--always-trust --lock-never \
-r [public key #1] -r [public key #2] \
-r [up to public key #20]  > user_disk_key.key
Copy the keys to the flash disk, and use them to build the new system (starting on the partitioning or adding LVM volumes pages)
Optional commands

You can verify the public keys are now part of the master keyring with:
gpg --no-default-keyring --homedir $GNUPGHOME --fin | less
Remotely encrypt a keylist file to another machine:
cat keylist.txt | ssh user@host "gpg --encrypt --armor \
--default-recipient-self > keylist.txt.asc"
List the keys used to encrypt the either one of the disk keys
cat disk_master.key | gpg --batch --decrypt --list-only \
--status-fd 1 2>/dev/null | awk '/^\[GNUPG:\] ENC_TO / { print $3 }'
Verify the user disk key file using one of the user keys
gpg --homedir . --always-trust --lock-never --list-packets user_disk_key.gpg
# or 
gpg --homedir . --always-trust --lock-never --decrypt --output /dev/null user_disk_key.gpg
