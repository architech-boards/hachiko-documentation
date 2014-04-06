Linux Kernel
============

As seen for **U-Boot** in the previous section, the first step is to get the kernel
sources, which in this guide we assume that they will be copied to directory:

.. host::

 /home/architech/Documents/kernel

You are free to choose the path you like the most for the kernel sources, just remember
to replace the path used in this guide with your custom path.
The suggested way, even for the kernel, to get the sources is to take them from the Yocto
build directory (copying them in the aforementioned directory to avoid messing up Yocto):

.. host::

 /home/architech/architech_sdk/architech/hachiko/yocto/build/tmp/work/hachiko64-poky-linux-gnueabi/linux/3.8.13-r2/linux-3.8.13/

or from the vanilla kernel tarball at this URL:

 `https://www.kernel.org/pub/linux/kernel/v3.0/patch-3.8.13.bz2 <https://www.kernel.org/pub/linux/kernel/v3.0/patch-3.8.13.bz2>`_.

and patch it using the patches found in Hachiko/SDRAM BSP meta-layer:

.. host::

 patch -p1 -d /home/architech/Documents/kernel < /home/architech/architech_sdk/architech/hachiko/yocto/meta-hachiko/recipes-kernel/linux/files/\*.patch

If you are not developing from within the official SDK, the most general solution to check
them out and patch the sources is:

.. host::

 | cd /home/architech/Documents
 | git clone -b dora https://github.com/architech-boards/meta-hachiko.git 
 | patch -p1 -d /home/architech/Documents/kernel < /home/architech/Documents/meta-hachiko/recipes-kernel/linux/files/\*.patch

To compile the kernel just execute these commands:

.. host::

 | cd /home/architech/Documents/kernel
 | source /home/architech/architech_sdk/architech/hachiko/toolchain/environment-nofs
 | make hachiko64_defconfig
 | make uImage dtbs

If you are not developing from within the original SDK, you are going to need to
:ref:`install the cross toolchain <manual_compilation_label>`.
The output of the compilation process is:

.. host::

 /home/architech/Documents/kernel/arch/arm/boot/uImage
 /home/architech/Documents/kernel/arch/arm/boot/dts/rza1-hachiko.dtb

