FAQ
^^^

Virtual Machine
===============

What is the password for the default user of the virtual machine?
-----------------------------------------------------------------

The password for the default user, that is **architech**, is:

::

  architech

What is **sudo**?
-----------------

**sudo** is a program for Unix-like computer operating systems that allows users to run programs/commands
with the security privileges of another user, normally the superuser or root. Not all the users can call
sudo, only the **sudoers**, **architech** (the default user of the virtual machine) user is a sudoer.
When you run a command preceeded by sudo Linux will ask you the user password, for **architech** user the
password is **architech**.

What is the password for user root?
-----------------------------------

By default, Ubuntu 12.04 32bit comes with no password defined for **roor** user, to set it run the following
command:

::

  sudo passwd root

Linux will ask you (twice, the second time is just for confirmation) to write the password for user root.

Hachiko/SDRAM
=============

How to switch from hachiko to hachiko/SDRAM and viceversa
---------------------------------------------------------

Switching from hachiko to hachiko/SDRAM or viceversa (adding or removing the external SDRAM) is a delicate operation that involves flashing a new U-Boot and using a new Kernel. To flash a new U-Boot it is needed to follow Section :ref:`flashing_NOR`, writing the new U-Boot from the U-Boot itself or from Linux. After the first boot with the new U-Boot it is always suggested to give the following commands to reset the U-Boot environment stored in the second partition of the serial NOR:

::

		env default -a
		saveenv 

and reboot the board.

Also the Linux kernel must be recompiled for the new configuration and substituted to the old one in the USB pend drive or in the NOR.

Bootargs and Framebuffer
------------------------

Hachiko kernel uses two parameters passed by U-Boot at boot time to enable / disable / configure the framebuffer. These two parameters are vdc5fb0 for channel 0 and vdc5fb1 for channel 1 of the video display controller.

The default value for these parameters is:

**hachiko/SDRAM**:

::

	vdc5fb0=3 vdc5fb1=4

**Hachiko**:

::

	vdc5fb0=0 vdc5fb1=0

The meaning of the values are here reported:

Channel 0 (**vdc5fb0**):

	* (**0**) unuse

	* (**1**) / (**2**) reserved for RZARSK dev board

	* (**3**) LCD parallel out enabled

Channel 1 (**vdc5fb1**):

	* (**0**) unuse

	* (**1**) / (**2**) / (**3**) reserved for RZARSK dev board

	* (**4**) LVDS output

It is possible to modify the default setting in U-Boot with the command:

::

	env set fbparam vdc5fb0=$B vdc5fb1=$A

with $A and $B the new set of parameters. To make the configuration permanent:

::

	saveenv

.. note::

	For the hachiko board without external SDRAM the usage of framebuffer can result in instability if not used with care. 

Change Framebuffer Resolution
-----------------------------

The default kernel shipped has the following default resolutions:

**LCD parallel out**: 

::

	480x272

**LVDS output**:

::

	800x480

to change them the file arch/arm/mach-shmobile/rskrza1-vdc5fb.c must be modified. Specifically the two structures containing the screen timings are:

**Channel 0 (parallel LCD)**:

::

		struct fb_videomode videomode_wqvga_lcd_kit

**Channel 1 (LVDS)**:

::

		struct fb_videomode videomode_lvds

