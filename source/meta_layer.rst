Meta Layer
==========

A Yocto/OpenEmbedded meta-layer is a directory that contains recipes,
configuration files, patches, and others things all needed by Bitbake to
properly "see" and build a BSP, a distrubution, and a (set of) package(s).

meta-hachiko is stored in repository git that can be cloned with:

::

	git clone -b dora https://github.com/architech-boards/meta-hachiko.git

For information about Yocto, Bitbake and the directories tree inside the
meta-layer, please refer to the Yocto documentation.

The Hachiko meta-layer defines two different machines: hachiko and hachiko64,
the latter to be used when an external SDRAM is available on the board.
Whereas hachiko64 machine can be used for virtually any distro and image
available on Yocto, hachiko machine must use a tailored custom distro and image
to be able to fit in the limited amount of SRAM available on-chip.

More specifically to create a minimal rootfs for the hachiko machine, local.conf
file must contain the following variables:

::

	MACHINE = "hachiko"
	DISTRO = "tiny-linux-uclibc"

and only the image tiny-image is supported and guaranteed to create a small
enough image to fit in less than 10MB SRAM. 

Yocto creates several output files for each machine. Here the most impontant
files used to create the rootfs:

**Hachiko**: (in $BUILDDIR/tmp/deploy/images/hachiko/)

	* *uImage* (Linux Kernel)
	* *uImage-rza1-hachiko.dtb* (DTB)
	* *u-boot.bin* (U-Boot)
	* *tiny-image-hachiko.tar.bz2* (Compressed rootfs)

**Hachiko64**: (in $BUILDDIR/tmp/deploy/images/hachiko64/)

	* *uImage* (Linux Kernel)
	* *uImage-rza1-hachiko.dtb* (DTB)
	* *u-boot.bin* (U-Boot)
	* *$IMAGE_NAME.tar.bz2* (Compressed rootfs)

