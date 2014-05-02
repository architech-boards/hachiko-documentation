To deploy the root file system, you are going to need an USB flash drive.

.. warning::

 The USB flash drive content will be lost forever!

1. Align a new partition to the first sector of your USB device:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'deploy_rootfs_rst-host-51' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="deploy_rootfs_rst-host-51" class="language-markup">sfdisk /path/to/your/USB/device &lt;&lt; EOF
 0,
 EOF</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>


2. Format it as an *EXT2* partition:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'deploy_rootfs_rst-host-52' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="deploy_rootfs_rst-host-52" class="language-markup">mkfs.ext2 /path/to/your/USB/device/partition</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

3. Extract your image inside such a media:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'deploy_rootfs_rst-host-53' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="deploy_rootfs_rst-host-53" class="language-markup">sudo tar -xjf /home/architech/architech_sdk/architech/hachiko/yocto/build/tmp/deploy/images/hachiko64/core-image-minimal-dev-hachiko.tar.bz2 -C /path/to/usb/media</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

4. Copy the kernel to the USB flash drive:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'deploy_rootfs_rst-host-54' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="deploy_rootfs_rst-host-54" class="language-markup">cp /home/architech/architech_sdk/architech/hachiko/yocto/build/tmp/deploy/images/hachiko64/uImage /path/to/usb/media/boot</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>

5. Now copy the device tree:

.. raw:: html

 <div>
 <div><b class="admonition-host">&nbsp;&nbsp;Host&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'deploy_rootfs_rst-host-55' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="deploy_rootfs_rst-host-55" class="language-markup">cp /home/architech/architech_sdk/architech/hachiko/yocto/build/tmp/deploy/images/hachiko64/uImage-rza1-hachiko.dtb  /path/to/usb/media/boot/rza1-hachiko.dtb</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>


6. Unmount the flash drive from your system

7. Insert the USB Flash drive in the USB port at the bottom of the USB connector of Hachiko/SDRAM board