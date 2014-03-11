.. _bsp_bootloader_label:

Hachiko BSP 
===========

The Board Support Package is composed by a set files, patches, recipes, configuration files, etc. This chapter gives you the information you need when you want to customize something, fix a bug, or simply learn how the all thing has been assembled.

Where not otherwise specified, the following sections are to be intended as common for Haciko and Hachiko/SDRAM.

User-specific paths and settings are indicated using the bash-variable notation $VAR with VAR being a straightforward name.

U-boot
======

The bootloader used by Hachiko boards is **U-Boot**. If you need to modify the bootloader or recompile it you have two ways to get the sources:

* using the sources in Yocto (after having compiled at least once the yocto image)
* using the official u-boot release patched with BSP patches for the hachiko board

It never advisable to work with the sources in the Yocto build directory, if you really want to modify the source code inside the Yocto environment we strongly suggest to refer to the official Yocto documentation. To avoid messing up Yocto recipes and installation, it is desirable to copy the patched u-boot sources you can find in the build directory elsewhere. U-Boot sources in Yocto can be found here:

**Hachiko**: 

::
	
	$BUILDDIR/tmp/work/hachiko-poky-linux-uclibceabi/u-boot/2013.04-r0/u-boot-2013.04/
	
**Hachiko/SDRAM**: 

::

	$BUILDDIR/tmp/work/hachiko64-poky-linux-gnueabi/u-boot/2013.04-r0/u-boot-2013.04/

The second way is to get the official U-Boot sources and patch them with the Hachiko BSP patches. Hachiko board uses the U-Boot 2013.04 that can be downloaded from:

`ftp://ftp.denx.de/pub/u-boot/u-boot-2013.04.tar.bz2 <ftp://ftp.denx.de/pub/u-boot/u-boot-2013.04.tar.bz2>`_.

otherwise a git repository is available for cloning:

::

	git clone -b v2013.04 http://git.denx.de/u-boot.git

Patches are to be found in the Yocto meta-layer Hachiko. To checkout the meta-layer and patch the U-Boot sources:

::

	git clone -b dora https://github.com/architech-boards/meta-hachiko.git 
	patch -p1 -d $UBOOT_DIR < $YOCTO/meta-hachiko/recipes-bsp/u-boot/files/*.patch

Configuration and board files for Hachiko board are in:

::

	board/renesas/hachiko/*
	include/configs/hachiko.h

Suppose you modified something and you want to recompile the sources to test your patches, well, you need a cross-toolchain. If you are not working with the virtual machine, the most comfortable way to get the toolchain is to ask *Bitbake* for it:

::

    bitbake meta-toolchain

When *Bitbake* finishes, you will find an install script under directory:

::

    path/to/build/tmp/deploy/sdk/

Install the script, and you will get under the installation directory a script to source to get your environment almost in place for compiling. The name of the script is:

**hachiko**:
::

    environment-setup-cortexa9hf-vfp-neon-poky-linux-ucilibceabi

**hachiko/SDRAM**:
::

	environment-setup-cortexa9hf-vfp-neon-poky-linux-ugnueabi

Anyway, the environment is not quite right for compiling the bootloader and the Linux kernel, you need to unset a few variables:

::

    unset CFLAGS CPPFLAGS CXXFLAGS LDFLAGS

Ok, now you a working environment to compile *u-boot*, just do:

::

	cd $UBOOT
	make mrproper

**hachiko**: 

::

	make hachiko
	
**hachiko/SDRAM**: 

::

	make hachiko64
	make

Once the build process is completed you will find the u-boot.bin file in the $UBOOT directory.

