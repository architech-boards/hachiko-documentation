Linux Kernel
============

As seen for **U-Boot** in the previous section the first step is to get the kernel
sources. As seen before the suggested way is to get them for the Yocto build
directory (copying them elsewhere to avoid messing up Yocto) or starting from a
vanilla kernel and patching it with the BSP patches taken from the Yocto
meta-layer.
Already patched sources can be found in the Yocto build directory:

**Hachiko**:

::

	$BUILDDIR/tmp/work/hachiko-poky-linux-uclibceabi/linux/3.8.13-r2/linux-3.8.13/
	
**Hachiko/SDRAM**:

::

	$BUILDDIR/hachiko64/tmp/work/hachiko64-poky-linux-gnueabi/linux/3.8.13/

Otherwise (patching the vanilla kernel) you can download the kernel from:

`https://www.kernel.org/pub/linux/kernel/v3.0/patch-3.8.13.bz2 <https://www.kernel.org/pub/linux/kernel/v3.0/patch-3.8.13.bz2>`_.

and patching it using the BSP patches found in the Hachiko meta-layer:

::

	patch -p1 -d $KERNEL_DIR < $YOCTO/meta-hachiko/recipes-kernel/linux/files/*.patch

BSP patches define two different defconfig files for Hachiko using internal RAM
and Hachiko with external RAM support (64MB):

**Hachiko**: 

::

	hachiko_defconfig
	
**Hachiko/SDRAM**: 

::

	hachiko64_defconfig

To compile the kernel:

::

	make ARCH=arm $DEFCONFIG
	make ARCH=arm uImage dtbs

The output of the compilation are two files:

::

	arch/arm/boot/uImage
	arch/arm/boot/dts/rza1-hachiko.dtb

