.. _unboxing_label:

********
Unboxing
********

This amazing board gets the power directly from the USB, so it comes with no external power supply.

This is what the box looks like

.. image:: _static/unboxing_close.jpg


And this is the content of box

.. image:: _static/unboxing_open.jpg

The board itself has been programmed to boot to a Linux console of a really tiny distribution,
custom tailored to fit the 10MB of internal ram of Renesas's SoC.

Shall we power on the board for the first time? Of course!

.. include:: serial_console.rst

Give *root* to the login prompt:

.. board::

 hachiko64 login: root

and press *Enter*.

.. note::

 Sometimes, the time you spend setting up minicom makes you miss all the output that leads to the login and you see just a black screen, press *Enter* then to get the login prompt.

Enjoy!