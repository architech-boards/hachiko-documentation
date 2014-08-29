***
FAQ
***

Virtual Machine
===============

What is the password for the default user of the virtual machine?
-----------------------------------------------------------------

The password for the default user, that is **architech**, is:

.. host::

 architech

What is the password of **sudo**?
---------------------------------

The default passowrd of **architech** is **architech**. If you are searching more information about **sudo** command please refer to :ref:`sudo <sudo_info_label>` section of the :ref:`appendix <appendix_label>`.

What is the password for user root?
-----------------------------------

By default, Ubuntu 12.04 32bit comes with no password defined for **roor** user, to set it run the following
command:

.. host::

 sudo passwd root

Linux will ask you (twice, the second time is just for confirmation) to write the password for user root.

What are device files? How can I use them?
------------------------------------------

Please refer to :ref:`device files <device_files_label>` section of the :ref:`appendix <appendix_label>`.


I have problems to download the vm, the server cut down the connection
----------------------------------------------------------------------

The site has limitation in bandwith. Use download manager and do not try to speed up the download. If you try to download fastly the server will broke up your download.

Hachiko/SDRAM
=============

How to switch from hachiko to hachiko/SDRAM and viceversa
---------------------------------------------------------

Switching from hachiko to hachiko/SDRAM or viceversa (adding or removing the external SDRAM) is a delicate operation that involves flashing a new U-Boot and using a new Kernel. To flash a new U-Boot it is needed to follow Section :ref:`flashing_NOR`, writing the new U-Boot from the U-Boot itself or from Linux. After the first boot with the new U-Boot it is always suggested to give the following commands to reset the U-Boot environment stored in the second partition of the serial NOR:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'faq_rst-board-291' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="faq_rst-board-291" class="language-markup">env default -a
 saveenv</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

and reboot the board.

Also the Linux kernel must be recompiled for the new configuration and substituted to the old one in the USB pend drive or in the NOR.

Bootargs and Framebuffer
------------------------

Hachiko kernel uses two parameters passed by U-Boot at boot time to enable / disable / configure the framebuffer. These two parameters are vdc5fb0 for channel 0 and vdc5fb1 for channel 1 of the video display controller.

The default value for these parameters is:

**hachiko/SDRAM**:

.. board::

 vdc5fb0=3 vdc5fb1=4

**Hachiko**:

.. board::

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

.. board::

 env set fbparam vdc5fb0=$B vdc5fb1=$A

with $A and $B the new set of parameters. To make the configuration permanent:

.. board::

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