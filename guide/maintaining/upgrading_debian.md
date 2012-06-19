Currently (April 2007), the current version of the debootstrap package does not support etch, the current stable release of Debian. This means that if you want the latest and greatest when building a system via debootstrap, you have to do a dist-upgrade yourself. Below is the step-by-step process...
Use dpkg --audit to verify that you do not have packages that are only halfway installed
Make a backup of your /etc/apt/sources.list, then change the file so that all references to sarge now say etch
(Optional) You can record the upgrade session (to capture errors to a log file) by using the following command: script -t 2>~/upgrade-etch.time -a ~/upgrade-etch.script
Run aptitude update to update the apt package lists for etch
Run aptitude upgrade followed by aptitude install initrd-tools; the first command upgrades your system, the second command upgrades important libraries that the system needs prior to performing any other tasks
Run aptitude dist-upgrade to bring the rest of the system up to the new version of Debian
Once the dist-update is complete, run aptitude update again in order to download and use the new package signature information
