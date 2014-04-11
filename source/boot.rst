Bootstrap
=========

In the latest release only bootmode 3 is supported by Hachiko/SDRAM board. That is the board boots from the serial flash memory connected to the SPI multi I/O bus.
To enable bootmode 3 the jumpers J4, J5 and J6 must be respectively in position 0, 1 and 1. When set for bootmode 3, the bootloader (U-Boot) must be flashed on the serial NOR at address 0x18000000 (SPI multi I/O bus space).
Hachiko/SDRAM boards already have the latest version of U-Boot flashed on SPI NOR at factory time.

For more information:

* `Hachiko schematics <http://downloads.architechboards.com/hachiko/doc/RSR977B.pdf>`_
* `RZ/A1H User's Manual <http://downloads.architechboards.com/hachiko/doc/r01uh0403ej0060_rz_a1h.pdf>`_

.. _flashing_NOR:

Write to NOR
============

| If for any reason U-Boot is corrupted or broken or simply the board refuses to boot again, the only way to recover the board is to try to flash a new U-Boot binary on the serial NOR. Flashing the NOR without U-Boot or Linux on the board being accessible is an easy task with the correct setup.
| Renesas already has brief information about how to use a PARTNER-Jet or ULINK2 to write U-Boot in serial flash. Follows an extract from Renesas documentation.

Using PARTNER-Jet
-----------------

* Write to serial flash:
	1. Execute PARTNER-Jet_ARM_V5_71b.exe
	2. Create a new project
	3. Copy "JETARM.CFG" and "usrflash_arm.MON" from flash directory
	   to created project.
	4. Push power on button of PARTNER-JET
	5. Write U-boot image data for serial flash boot to 0x18000000.


Using ULINK2
------------

To use ULINK2, DS-5 from ARM Ltd. and binary writing tool provided from Renesas are needed. As for the way to obtain these tools, please ask Renesas electronics sales representative. After the installation of DS5 and binary writing tool, please go to the next step.

* Writing to serial flash:
	1. Clear *RZ_A1H_spibsc_boot_init\\Debug\\RZ_A1H_spibsc_boot_init.bin\\* directory in DS-5 workspace. 
	2. Rename U-boot image file for serial flash boot *u-boot.bin* as *VECTOR_TABLE* and put it on the above directory.
	3. Run DS-5.
	4. Connect ULINK2 to the board and poweron the board.
	5. Click script tag and double click *RZ_A1H_sflash_boot_user.ds*

| The process using the ULINK2 is described here more in detail. 
|
| Software you need:

* *SPIBSCBoot.zip* file containing the DS-5 workspace, the script file and a launcher configuration 
  (`download link <http://downloads.architechboards.com/hachiko/doc/SPIBSCBoot.zip>`_)
* DS-5 (preferred and tested version: *DS500-BN-00003-r5p0-15rel1*)

Setup of the environment and first run:
	1. Install the DS-5 and use the activation code found here `http://ds.arm.com/renesas/rza-starter-kit/ <http://ds.arm.com/renesas/rza-starter-kit/>`_ to enable the Renesas compiler when a valid licence is asked the first time DS-5 is started
	2. Unzip the content of *SPIBSCBoot.zip*. Three more files should be available: *Connection_Only.launch*, *Workspace.zip* and *script.zip*. Unzip also *Workspace.zip* and *script.zip*
	3. Start DS-5 and import into workspace all the three projects ([File] >> [Import Existing Projects into Workspace]) taking care of select [Copy projects into workspace] when importing
	4. Import also the launch configuration (file *Connection_Only.launch*) ([File] >> [Import] >> [Launch configurations])
	5. Copy the script directory into workspace directory and copy only the script *RZ_A1H_sflash_boot_user.ds* in the workspace directory (outside the script dir)
	6. Open [Run] >> [Debug Configurations...] menu, then click on [DS-5 Debugger] >> [Connction_only]. In the [Connection] tab select [Renesas]/[RZ/A Series-R7S72100]/[Bare Metal Debug]/[Debug of Coretex-A9 via ULINK2] and press the [Browse] button to identify and select the attached debugger. Press Apply to save changes.
	7. Rename *u-boot.bin* file as *VECTOR_TABLE* and copy it in *RZ_A1H_spibsc_boot_init\\Debug\\RZ_A1H_spibsc_boot_init.bin\\* directory inside the workspace
	8. Connect the ULINK2 and power on the board. Select again [Run] >> [Debug Configurations...] menu, click on [DS-5 Debugger] >> [Connction_only] and on the Debug button. 
	9. When the DS-5 Dubug perspective is open, in the [Script] tab of DS-5 import the script *RZ_A1H_sflash_boot_user.ds*
	10. Pause the microprocessor with the button in the toolbar and double-click on the script to run it
	11. When the download script runs, the flash downloader is launched and a message is shown in the [Application Console]. Answer Y to the first question and N to the second one
	12. When downloading finishes, it is possible to disconnect from target

For all the subsequent times it will be enough:
	1. Open the DS-5
	2. Switch to DS-5 Debug perspective
	3. Select the launcher from the [Debug control] tab
	4. Press the [Connect to Target] button in the toolbar
	5. Double click on the script in the [Script] tab
	6. Answer Y and N to the two questions in the [Application Console]
	7. Disconnect from the target using the [Disconnect from the Target] button in the toolbar

For a more detailed description of the entire process, see:

* Renesas Application Note: `Example of Downloading to Serial Flash Memory Using the Semihost Function of ARM DS-5" <http://downloads.architechboards.com/hachiko/doc/RZ_A1H_sflash_sample_rev0.01e.pdf>`_
* Renesas Applicaton Note: `Example of Booting from Serial Flash Memory <http://downloads.architechboards.com/hachiko/doc/RZ_A1H_spibscboot_sample_rev0.01e.pdf>`_ 

.. note::

 | * To change file to program in serial NOR it is enough to overwrite again the file named *VECTOR_TABLE* as seen at step (8)
 | * It is not possible to reprogram the NOR flash if the board successfully boots Linux. To be able to write the NOR using the DS-5 it is required that the board stops in U-Boot or at first stage bootloader (before U-Boot is loaded)
 

U-Boot
======

To check if the board is correctly programmed, connect the board to a terminal emulator on your computer. To see how this can be achieved please refer to section `serial_console_label`.

If everything is setup correctly you should be able to see the bootstrap process and the U-Boot output. In particular, as soon as the board is powered a countdown is started and displayed on the serial output. If a key is pressed before the countdown expires the autoboot stops, otherwise Linux is loaded from USB or SPI NOR.

On Hachiko/SDRAM board you can boot using the USB or the serial NOR. During the boot process, if U-Boot detects a correct kernel and rootfs on the USB drive it will boot from this USB device, otherwise it will switch to SPI NOR. In case no correct linux kernel is detected, the boot stops in the U-Boot console.

For a brief documentation about U-Boot:

* `Renesas U-Boot documentation: <http://downloads.architechboards.com/hachiko/doc/users_manual_u-boot_E.txt>`_ 

Boot from USB
=============

Booting from USB requires that an USB pen drive is prepared with all the files
needed for booting Linux and that it is correctly partitioned.

.. important::

 The only USB port that it is possible to use for booting is the USB port at the bottom of the USB connector.

USB partitioning
----------------

The USB pen driver is required to have one single *EXT2* partition with a start
sector of the partition below the 63rd sector. It is possible to use tools as
*fdisk* or *cfdisk* to partition the USB drive.

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-host-11' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-host-11" class="language-markup">cfdisk /path/to/your/USB/device</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

As alternative it is possible to use the sfdisk tools to have the partition
correctly aligned to the first sector:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-host-12' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-host-12" class="language-markup">sfdisk /path/to/your/USB/device &lt;&lt; EOF
 0,
 EOF</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

| To format the partition it is enough:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-host-13' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-host-13" class="language-markup">mkfs.ext2 /path/to/your/USB/device/partition</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>


USB content
-----------

When booting from USB, U-Boot expects to find a valid single *EXT2* partition in the USB pen drive containing the rootfs. Moreover U-Boot needs to find in  */boot* directory a valid kernel image and a valid *DTB* file respectively named *uImage* and *rza1-hachiko.dtb*.

When using Yocto to generate the rootfs we need to extract the compressed rootfs found in

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-host-14' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-host-14" class="language-markup">/home/architech/architech_sdk/architech/hachiko/yocto/build/tmp/deploy/images/hachiko64</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

in the partition on the USB and copy the kernel in */boot/uImage* and *DTB* file in */boot/rza1-hachiko.dtb*.

Briefly, to have a bootable USB stick after having compiled an image with Yocto:

	1. Create one *EXT2* partition in the USB stick
	2. Extract the content of your *.tar.bz2* rootfs tarball file in the *EXT2* partition
	3. Copy *uImage* to */boot*
	4. Copy *uImage-rza1-hachiko.dtb* to */boot* and rename it as *rza1-hachiko.dtb*

At this point it is possible to boot Linux by inserting the USB pen drive in the correct USB port and power on the board.

Boot from NOR
=============

When no USB device is attached or the kernel image is not valid, U-Boot tries to boot from SPI NOR. In Hachiko/SDRAM board the NOR is required to contain all the needed files in the first serial flash memory on channel 0.

.. note::

 A valid NOR Linux image is programmed at factory time in Hachiko/SDRAM NOR, so it is possible to start using Hachiko/SDRAM board right away.

NOR Partitioning 
----------------

The serial flash memory is divided into 5 partitions according to the following scheme (the base address is 0x18000000):

::

	0x18000000-0x18080000 spibsc0_loader  (offset: 0x00000000)
	0x18080000-0x180c0000 spibsc0_bootenv (offset: 0x00080000)
	0x180c0000-0x184c0000 spibsc0_kernel  (offset: 0x000c0000)
	0x184c0000-0x18500000 spibsc0_dtb     (offset: 0x004c0000)
	0x18500000-0x1c000000 spibsc0_rootfs  (offset: 0x00500000)

::

	spibsc0_loader: contains u-boot (u-boot.bin)
	spibsc0_bootenv: contains u-boot environment
	spibsc0_kernel: contains the Linux kernel (uImage)
	spibsc0_dtb: contains the DTB file (rza1-hachiko.dtb)
	spibsc0_rootfs: contains the rootfs

NOR content
-----------

To write in NOR and replace or update the content of the NOR partitions you can go through U-Boot or Linux. It is strongly recommended to use Linux for writing new data in NOR partitions, especially when no external SDRAM is available.

Using U-Boot [not recommended]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Using U-Boot for writing or updating data in SPI NOR is not advisable especially when no external SDRAM is available. 

.. warning::

 The operation is prone to failure, use it at your own risk.

The process of writing data in serial NOR using U-Boot goes through 3 main steps: 1) load the file to write in a temporary RAM location, 2) erase data on the NOR partition and 3) write the new data.

1. Assuming you have the file on the USB pen drive you have to load it in RAM using the following commands:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-221' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-221" class="language-markup">usb start
 ext2load usb 0 0x0c000000 /path/to/your/u-boot.bin</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

The output from the command is also the size of the file loaded, info useful for step (3).

RAM ranges:

::

 0x0c000000 - 0x10000000
	
Please, note that while *spibsc0_loader*, *spibsc0_kernel*, and *spibsc0_dtb* partitions of the flash memory contain raw data respectively for *u-boot.bin*, *uImage* and *rza1-hachiko.dtb*, whereas partition *spibsc0_rootfs* contains raw data for the filesystem that, in our case, is a *JFFS2* image. That is, when writing a new rootfs in *spibsc0_rootfs* partition it is needed to use the image file *.jffs2* generated by Yocto. 

2. To erase data on the NOR partition;

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-222' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-222" class="language-markup">sf erase $OFFSET $SIZE</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

where $OFFSET is the partition offset and $SIZE its size in bytes.

3. To write new data:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-223' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-223" class="language-markup">sf write $RAM_ADDR $OFFSET $SIZE</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

where $RAM_ADDR is the temporary RAM location holding our file (tipically 0x0c000000), $OFFSET is the partition offset and $SIZE is the file size in bytes as obtained by the output of the comman ext2load in step (1).

For more informations about flash managing with U-Boot refer to:

* `Renesas U-Boot documentation <http://downloads.architechboards.com/hachiko/doc/users_manual_u-boot_E.txt>`_ 

Using Linux
^^^^^^^^^^^

To use linux for writing or updating data on the serial NOR you are going to need **MTD utils**. It is possible to compile a small image containing the MTD utils with Yocto by means, for example, of image *core-image-minimal-mtdutils* that can be generated by *Bitbake* with this command line:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-host-15' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-host-15" class="language-markup">bitbake core-image-minimal-mtdutils</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

In Linux, the process is made easier by the MTD framework that remap each NOR partition to a different device file. In particular:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-224' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-224" class="language-markup">/dev/mtd0: spibsc0_loader
 /dev/mtd1: spibsc0_bootenv
 /dev/mtd2: spibsc0_kernel
 /dev/mtd3: spibsc0_dtb
 /dev/mtd4: spibsc0_rootfs</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

Again the process goes through 2 steps: (1) erasing the content of the serial NOR partition and (2) write the new data.

1. To erase the content of the partition the tool flash_erase can be used. For raw files as *u-boot.bin*, *uImage* or *rza1-hachiko.dtb*, the tool can be used as follow:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-225' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-225" class="language-markup">flash_erase /path/to/your/mtd/device 0 0</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

This command completely erases the content of the partition. For the root file system the command is slightly different, since being *spibsc0_rootfs* a *JFFS2* partition, it requires proper formatting, so for mtd4 device you need to run this command:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-226' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-226" class="language-markup">flash_erase -j /dev/mtd4 0 0</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

2. To write the new data on the serial NOR the tool flashcp is used. Again for raw file the simple syntax is:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-227' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-227" class="language-markup">flashcp -v /path/to/your/file /path/to/your/mtd/device</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

For rootfs we have two different ways to write data in *spibsc0_rootfs* partition:

1. Using the image file *.jffs2* generated by Yocto

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-228' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-228" class="language-markup">flashcp -v /path/to/your/image/file.jffs2 /dev/mtd4</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

2. If you wish to use the image file *.tar.bz2* instead, you need to mount the partition and decompress the file content in place.

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'boot_rst-board-229' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="boot_rst-board-229" class="language-markup">mount -t jffs2 mtd4 /mnt/
 tar xv -C /mnt/ -f /path/to/your/image/file.tar.bz2
 umount /mnt/</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

For more information on how to manage flash storage with Linux:

  `http://free-electrons.com/blog/managing-flash-storage-with-linux/ <http://free-electrons.com/blog/managing-flash-storage-with-linux/>`_