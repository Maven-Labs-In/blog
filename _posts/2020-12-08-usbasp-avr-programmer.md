---
title: USBasp - AVR programmer
date: 2020-12-08 14:56:45 +/-0530
categories: [programmer, avr]
tags: [usbasp]     # TAG names should always be lowercase
---

## USBasp - Open source AVR programmer
### Construction

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; To tinker with any microcontroller, first step is to transfer/program the software to the microcontroller. There are different ways of doing this, listed as below options.
1. Silicon manufacturer programmers/debuggers:
   - High End  - [ATMEL-ICE](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/ATATMEL-ICE)
   - Mid range - [Pickit4](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/PG164140)
   - Low cost  - [Snap](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/PG164100)
2. Bootloaders - Special program that reside's inside microcontroller and handles the programming! I wont be considering bootloaders as of now, this is a task best to be done in future ;)
3. Manufacturer demo boards with built in debuggers - [ATMEGA328P-XMINI](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/ATMEGA328P-XMINI)
4. Other(Open source) programmers
   - [USBasp](https://www.fischl.de/usbasp/)
   - [USBtinyisp](https://learn.adafruit.com/usbtinyisp)
   - [Pocket AVR Programmer](https://www.sparkfun.com/products/9825)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The theme of this blog is DIY, so let's build our own copy of the open source AVR programmer from Thomas Fischl [USBasp](https://www.fischl.de/usbasp/).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Below schematic of USBasp programmer is based on original [USBasp](https://www.fischl.de/usbasp/), However i made few changes to the schematic as described here
   - Level translator based on 74125 QUAD buffers.
   - Addition of a I2C EEPROM for future standalone operation mode i am planning.
   - Addition of a new JP2 jumper to trigger standalone operation.
   - Addition of a Jumper JP4 to power the programmer from target board instead of USB connection(Planned to be used in standalone mode to power the programmer)


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Modified schematic of USBasp described in this blog can be seen below.
![USBasp_sch](https://user-images.githubusercontent.com/75629402/102087480-ead71200-3e19-11eb-8884-068c5b7db494.png)
If you need a high resolution pdf copy of this schematic, Its available right here [USBasp_sch.pdf](https://github.com/Maven-Labs-In/blog/files/5689978/USBasp_sch.pdf)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Below you can find build images of programmer i built using only through whole components! I have omitted EEPROM and jumper to trigger standalone operation in the 1st version of the programmer constructed for simplicity :)

![USBasp_build1](https://user-images.githubusercontent.com/75629402/102102554-3eeaf200-3e2c-11eb-894c-504e75b3274a.png)

![USBasp_build2](https://user-images.githubusercontent.com/75629402/102102602-4d390e00-3e2c-11eb-8b53-cb151460a55f.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; You can find the software to be flashed into our shiny new USBasp on Fischl's [website](https://www.fischl.de/usbasp/usbasp.2011-05-28.tar.gz).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; !!!Happy tinkering!!! :D