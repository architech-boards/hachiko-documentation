Opkg
====

.. image:: _static/opkg.png
   :align: left

| 
| Opkg (Open PacKaGe Management) is a lightweight package management system. It is written in C and resembles apt/dpkg in operation. It is intended for use on embedded Linux devices and is used in this capacity in the OpenEmbedded and OpenWrt projects. 
| 
|

Useful commands:

- update the list of available packages:

.. board::

 opkg update

- list available packages:

.. board::

 opkg list

- list installed packages:

.. board::

 opkg list-installed 

- install packages:

.. board::

 opkg install <package 1> <package 2> ... <package n> 

- list package providing <file>

.. board::

 opkg search <file>

- Show package information

.. board::

 opkg info <package>

- show package dependencies:

.. board::

 opkg whatdepends <package> 

- remove packages:

.. board::

 opkg remove <package 1> <package 2> ... <package n>

Force Bitbake to install Opkg in the final image
------------------------------------------------

With some images, *Bitbake* (e.g. *core-image-minimal*) does not install the package management system in the final target.
To force *Bitbake* to include it in the next build, edit your configuration file

.. host::

 /home/architech/architech_sdk/architech/hachiko/yocto/build/conf/local.conf

and add this line to it:

::

 IMAGE_FEATURES_append = " package-management"


Create a repository
-------------------

**opkg** reads the list of packages repositories in configuration files located under */etc/opkg/*. 
You can easily setup a new repository for your custom builds:

1) Install a web server on your machine, for example **apache2**:

.. host::

 sudo apt-get install apache2

2) Configure apache web server to "see" the packages you built, for example:

.. host::

 sudo ln -s /home/architech/architech_sdk/architech/hachiko/yocto/build/tmp/deploy/ipk/ hachiko-ipk

3) Create a new configuration file on the target (for example */etc/opkg/my_packages.conf*) containing lines like this one to index the packages related to a particular machine:

.. board::

 src/gz hachiko64 http://192.168.0.100:8000/hachiko-ipk/hachiko64
 
To actually reach the virtual machine we set up a port forwarding mechanism in Chapter :ref:`vm_label` so that every time the board communicates with the workstation on port 8000, VirtualBox actually turns the communication directly to the virtual machine operating system on port 80 where it finds *apache* waiting for it.

4) Connect the board and the personal computer you are developing on by means of an ethernet cable

5) Update the list of available packages on the target

.. board::

 opkg update 

Update repository index
-----------------------

Sometimes, you need to force bitbake to rebuild the index of packages by means of:

.. host::

 bitbake package-index
