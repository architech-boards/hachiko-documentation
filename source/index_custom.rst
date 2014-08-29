You can find the previous documentation: `Here <http://architechboards-hachiko-v101.readthedocs.org>`_

**Hachiko** board is a deeply embedded board based on Renesas's RZ/A1 processor, which has a huge internal random access memory, enough to run a Linux distribution directly without the help of memory external to the SoC. 
The distribution you can run from within the internal RAM must be custom tailored for the final application. If you want to have more flexibility, the board allows you to mount an external memory chip so you can extend the amount of system memory.
From the point of view of the SDK, there are differences regarding Linux kernel configuration, cross-toolchain, how the Linux distribution has to be composed, etc., hence there are two different SDKs for the two different configurations and two different documentations.

.. important::

 We will call **Hachiko/SDRAM** the configuration that employs external memory, while we will call **Hachiko** the configuration without it.

 This documentation is about **Hachiko/SDRAM**.

Have you just received your Hachiko/SDRAM board? Then you sure want to read the :ref:`unboxing_label` chapter first.

