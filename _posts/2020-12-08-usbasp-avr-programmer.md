---
title: USBasp - AVR programmer
date: 2020-12-08 14:56:45 +/-0530
categories: [programmer, avr]
tags: [usbasp]     # TAG names should always be lowercase
---

## USBasp - Open source AVR programmer
### Construction

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; To tinker with any microcontroller, first step is to transfer/program the software to the microcontroller. There are different ways of doing this as below.
1. Silicon manufacturer programmers/debuggers:
   - High End  - [ATMEL-ICE](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/ATATMEL-ICE)
   - Mid range - [Pickit4](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/PG164140)
   - Low cost  - [Snap](https://www.microchip.com/DevelopmentTools/ProductDetails/PartNO/PG164100)
2. Bootloaders - Special program that resides inside microcontroller and handles the programming! I wont be considering bootloaders as of now, this is a task best to be done in future ;)
3. Other(Open source) programmers
   - [USBasp](https://www.fischl.de/usbasp/)
   - [USBtinyisp](https://learn.adafruit.com/usbtinyisp)
   - [Pocket AVR Programmer](https://www.sparkfun.com/products/9825)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The theme of this blog is DIY, so let's build our own copy of the open source AVR programmer from Thomas Fischl [USBasp](https://www.fischl.de/usbasp/).
