Root FS
=======

By default, Hachiko/SDRAM Yocto/OpenEmbedded SDK will generate two different types of files when you build an image:

* *\*.tar.bz2*, and

* *\*.jffs2*

The *.tar.bz2* file can be expanded in your final medium partition (flash memory or USB stick) or on your host development system and used for build purposes with the Yocto Project.
File *.jffs2* can be written out "as is" on the final medium (usually flash NOR partition) with, for example, *dd* program:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'rootfs_rst-host-141' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="rootfs_rst-host-141" class="language-markup">sudo dd if=/path/to/image.jffs2 of=/path/to/your/USB/device</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

or 

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'rootfs_rst-board-271' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="rootfs_rst-board-271" class="language-markup">flashcp -v /path/to/image.jffs2 /dev/mtd4</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

.. important::

 Be very careful when you use *dd* or *flashcp* to write to a device to pick up the right device