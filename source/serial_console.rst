On Hachiko/SDRAM there is the dedicated serial console connector **CN2**

.. image:: _static/board-cn2.jpg

which you can connect, by means of a mini-USB cable, to your personal computer.

.. note::

 Every operating system has its own killer application to give you a serial terminal interface. In this guide, we are assuming your **host** operating system is **Ubuntu**.

On a Linux (Ubuntu) host machine, the console is seen as a ttyUSBX device and you can access to it by means
of an application like *minicom*.

.. host::

 sudo minicom -ws

If minicom is not installed, you can install it with:

.. host::

 sudo apt-get install minicom

If on your system the device has been recognized as **ttyUSB0**
then you can setup your port with these parameters:

::

    +-----------------------------------------------------------------------+
    | A -    Serial Device      : /dev/ttyUSB0                              |
    | B - Lockfile Location     : /var/lock                                 |
    | C -   Callin Program      :                                           |
    | D -  Callout Program      :                                           |
    | E -    Bps/Par/Bits       : 115200 8N1                                |
    | F - Hardware Flow Control : No                                        |
    | G - Software Flow Control : No                                        |
    |                                                                       |
    |    Change which setting?                                              |
    +-----------------------------------------------------------------------+
            | Screen and keyboard      |
            | Save setup as dfl        |
            | Save setup as..          |
            | Exit                     |
            | Exit from Minicom        |
            +--------------------------+

otherwise, just replace *ttyUSB0* with the proper device.

Once you are done configuring the serial port, you are back to *minicom* main menu and you can select *exit*.

