The Sourceforge site has all of the files described below available for
downloading, as well as PDF copies of this documentation for offline usage.
The following files are available on the Sourceforge site:

A Project Naranja kernel and initramfs image (vmlinuz-*, initramfs-*.gz; this
can be used to boot and install a system onto a new machine as well as boot an
installed system for normal use or repair
Debian packages for the Linux kernel (linux-image-*.deb), as well as the
packages for the loop-aes encryption software (loop-aes-*.deb,
loop-aes-ciphers-*.deb). You would install the kernel and loop-aes packages
onto a system that has just been built using these instructions (specifically,
a system that has just been built with debootstrap) so that when you reboot,
other modules not provided inside of the initramfs image are available for
loading by the kernel
PDF copies of these pages for offline use
You will need a Linux kernel, initramfs image to perform the initial install,
and the Debian kernel and loop-aes packages to boot into the new system from a
hard drive. The Debian kernel packages also contain other kernel modules that
are not loaded during system startup, so you'll need the kernel package
installed to be able to load the rest of the kernel modules that are needed
for the VIA EPIA boards.
