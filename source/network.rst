Network
=======

The network PHY is provided by Renesas's chip 60610 (*U6*).
Within Linux, you can see the network interface as **eth0**.

If you want that configuration to be brought up at boot you can add a few line in file */etc/network/interfaces*, for
example, if you want *eth0* to have a fixed ip address (say 192.168.0.10) and MAC address of value 1e:ed:19:27:1a:b6
you could add the following lines:

.. raw:: html

 <div>
 <div><b class="admonition-board">&nbsp;&nbsp;Board&nbsp;&nbsp;</b>&nbsp;&nbsp;<a style="float: right;" href="javascript:select_text( 'network_rst-board-251' );">select</a></div>
 <pre class="line-numbers pre-replacer" data-start="1"><code id="network_rst-board-251" class="language-markup">auto eth0
 iface eth0 inet static
     address 192.168.0.10
     netmask 255.255.255.0
     hwaddress ether 1e:ed:19:27:1a:b6</code></pre>
 <script src="_static/prism.js"></script>
 <script src="_static/select_text.js"></script>
 </div>
