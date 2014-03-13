Create USB image to bootup
==========================

You can decompress the image file in a USB Flash drive. Format it with EXT2 filesystem then:

::

	$ sudo tar -jxvf tmp/deploy/images/hachiko/minimal-dev.tar.bz2 -C /media/<usbkey_folder>

.. important::

	Remeber that if you are using an Hachiko without extarnl ram the image is named tiny-image-hachiko.tar.bz2

After that insert the USB Flash drive in the usb connector of the hachiko board. At the powerup the bootloader automatically will load the kernel and the rootfs from the USB even if there is an valid image in flash board.

