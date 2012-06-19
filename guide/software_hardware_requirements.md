Software Requirements

loop-aes (required for secure encrypted filesystems)
Linux kernel with Padlock encryption patches applied/included (required for
encrypted filesystems using faster hardware encryption)
rng-tools package (optional, speeds up random number generation when used with
any of the Linux kernel RNG modules)
Optional Software
Michal Ludvig's site also has patches for OpenSSH and OpenSSL, which enable
the Padlock engine for operations that can use hardware encryption in those
programs.

Hardware Requrements

A VIA motherboard with Padlock Encryption will give you the best overall
performance. Currently (March 2007) the EN, EX, and SP Series motherboards
have the Padlock Encryption Engine. The "Secure by Design" page on VIA's
website gives you a pretty table to look over if you wish. VIA releases new
motherboards regularly, any new motherboards should also contain the Padlock
Security Engine as well.
The EN and EX series motherboards have a C7 Esther processor; all VIA
processors that use the Esther core come with hardware encryption for AES, SHA
and better hardware random number generation
The SP series motherboards have a C3 Nehemiah processor; all Nehemiah
processors come with hardware encryption for AES and hardware random number
generation
The C3 processor is fully supported in the Linux kernel source, the C7 is
supported for the most part, you may have to apply patches from Michal
Ludvig's site to get the newer features working
You can use motherboard without the Padlock Encryption Suite, however, don't
expect anywhere near the same performance.
The following motherboard chipsets have some (but not all) of the features of
the VIA motherboard with Padlock Encryption:
Intel
i810 chipset (80802 RNG), Hardware RNG only
AMD
boards with a 768 Southbridge or a Geode LX chipsets, Hardware RNG only
Texas Instruments
OMAP CPU Family, Hardware RNG only
You could also set up something similar to what's described here with a
different hardware crypto accelerator (like a Hifn or SafeNet crypto board for
example, see Tom Dunigan's site for a very complete list of vendors), but
that's outside of the scope of this project. Soekris Engineering also has PCI
and Mini-PCI crypto boards based on the Hifn chipsets.
Got a question? Get answers at the project-naranja Google Groups page.
