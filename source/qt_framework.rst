.. _qt_framework_label:

Qt Framework
============

The Qt Framework used by this SDK is composed of libraries for your host machine and your target.
To compile the libraries for *x86* you only need your distribution toolchain, while to compile the
libraries for Hachiko/SDRAM board you need the proper cross-toolchain (see Chapter :ref:`manual_compilation_label`
for further information on how to get it).

This section just wants to show you how the framework has been generated.

Before to begin, keep in mind you might need to install the following package to compile yourself
the libraries under Ubuntu

.. host::

 | sudo apt-get install libxrender-dev

So, to install *qt-everywhere* for *x86* from sources, the usual drill of download, uncompress, *configure*,
*make* and *make install* is required:

.. host::

 | wget http://download.qt-project.org/official_releases/qt/4.8/4.8.5/qt-everywhere-opensource-src-4.8.5.zip
 | unzip qt-everywhere-opensource-src-4.8.5.zip
 | cd qt-everywhere-opensource-src-4.8.5
 | ./configure /\*Choose Open source Edition when asked, and accept the license\*/
 | make
 | make install 

The installation of the libraries for Hachiko/SDRAM from sources is sligthly more complicated. Once you downloaded
and uncompressed the sources

.. host::

 | wget http://download.qt-project.org/official_releases/qt/4.8/4.8.5/qt-everywhere-opensource-src-4.8.5.zip
 | unzip qt-everywhere-opensource-src-4.8.5.zip
 | cd qt-everywhere-opensource-src-4.8.5
 | cp -r mkspecs/qws/linux-arm-g++/ mkspecs/qws/linux-hachiko-g++
 | cd mkspecs/qws/linux-hachiko-g++/
 | gedit qmake.conf

you need to customize *qmake* configuration

.. host::

 | #
 | # qmake configuration for building with arm-linux-g++
 | #
 | 
 | include(../../common/linux.conf)
 | include(../../common/gcc-base-unix.conf)
 | include(../../common/g++-unix.conf)
 | include(../../common/qws.conf)
 | 
 | # modifications to g++.conf
 | QMAKE_CC                = arm-poky-linux-gnueabi-gcc --sysroot=/home/architech/architech_sdk/architech/hachiko/toolchain/sysroots/cortexa9hf-vfp-neon-poky-linux-gnueabi
 | QMAKE_CXX               = arm-poky-linux-gnueabi-g++ --sysroot=/home/architech/architech_sdk/architech/hachiko/toolchain/sysroots/cortexa9hf-vfp-neon-poky-linux-gnueabi
 | QMAKE_LINK              = arm-poky-linux-gnueabi-g++ --sysroot=/home/architech/architech_sdk/architech/hachiko/toolchain/sysroots/cortexa9hf-vfp-neon-poky-linux-gnueabi
 | QMAKE_LINK_SHLIB        = arm-poky-linux-gnueabi-g++ --sysroot=/home/architech/architech_sdk/architech/hachiko/toolchain/sysroots/cortexa9hf-vfp-neon-poky-linux-gnueabi
 | 
 | # modifications to linux.conf
 | QMAKE_AR                = arm-poky-linux-gnueabi-ar cqs
 | QMAKE_OBJCOPY           = arm-poky-linux-gnueabi-objcopy
 | QMAKE_STRIP             = arm-poky-linux-gnueabi-strip
 | 
 | load(qt_config)

save the file and exit from gedit, then *configure*, *make* and *make install*

.. host::

 | cd ../../../
 | ./configure -no-pch -opensource -confirm-license -prefix /usr/local/Trolltech/Hachiko -no-qt3support -embedded arm -nomake examples -nomake demo -little-endian -xplatform qws/linux-hachiko-g++ -qtlibinfix E
 | make
 | make install

A comfortable tool to get your job done with Qt is *Qt Creator*, which its use will be introduced
in Section :ref:`qt_creator_label`. You can download it from here:

.. tip::

 http://sourceforge.net/projects/qtcreator.mirror/files/Qt%20Creator%202.8.1/qt-creator-linux-x86-opensource-2.8.1.run/download
