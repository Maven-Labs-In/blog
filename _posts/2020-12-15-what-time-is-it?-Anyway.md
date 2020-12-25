---
title: What time is it?.....Anyway!!!
date: 2020-12-15 15:05:45 +/-0530
categories: [rtc, esp8266, nodemcu]
tags: [network time protocol]     # TAG names should always be lowercase
---

>“Time is an illusion.”
― Albert Einstein 

>"Timeplan doubly so."
― Anonymous

## What time is it?
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The need to know the time was a fundamental quest throughout human history. In day-to-day life, the clock is consulted for periods less than a day whereas the calendar is consulted for periods longer than a day. I often get up in the night and wonder what time is it? I have tried to use my phone as clock but whenever i see the mobile screen it ends up disturbing my sleep for long hours! I wonder what alternate options i have ;)

1. LED clock available comercially:
   - LED clocks  - [Digital Alarm Clock](https://www.amazon.com/Electronic-Settings-Humidity-Temperature-Electric/dp/B07RKTVQDR/ref=sr_1_3?dchild=1&keywords=led+clock&qid=1608107186&sr=8-3)

2. DIY digital clock 
   - MCU Timer based - Using built in timer HW in MCU to derive time. This method is not accurate due to Oscillator used to drive the MCU.
   - DS1307 based - Dedicated RTC IC which has built in HW capability to count time in , seconds, minutes, hours, days, month and years. Measurements have shown that the time drifts based on temperature effects(on 32KHz crystal) and aging effects. 
   - DS1321 based - More accurate than ds1307 due to the fact that temperature compensated cyrstal is built inside the IC itself. There is an option to run the calender/time with additional battery backup used when there is no power to the circuit. Measurements have shown that this method can loose few seconds per year.
   - Network Time Protocol(NTP) based clock - Robust time synchonization based on network synced time. Costly option compared to all the options above due the additional software and hardware capabilities needed to connect to Internet.

## And the winner is! ...... USBasp
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The theme of this blog is DIY, so let's build our own copy of the open source AVR programmer from Thomas Fischl [USBasp](https://www.fischl.de/usbasp/).

## Schematics and features
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Below schematic of USBasp programmer is based on original [USBasp](https://www.fischl.de/usbasp/), However i made few changes to the schematic as described here
   - Level translator based on 74125 QUAD buffers.
   - Addition of a I2C EEPROM for future standalone operation mode i am planning.
   - Addition of a new JP2 jumper to trigger standalone operation.
   - Addition of a Jumper JP4 to power the programmer from target board instead of USB connection(Planned to be used in standalone mode to power the programmer)


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Modified schematic of USBasp described in this blog can be seen below.
![USBasp_sch](https://user-images.githubusercontent.com/75629402/102087480-ead71200-3e19-11eb-8884-068c5b7db494.png)
If you need a high resolution pdf copy of this schematic, Its available right here [USBasp_sch.pdf](https://github.com/Maven-Labs-In/blog/files/5689978/USBasp_sch.pdf)

## Build images
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Below you can find build images of programmer i built using only through whole components! I have omitted EEPROM and jumper to trigger standalone operation in the 1st version of the programmer constructed for simplicity :)

![USBasp_build1](https://user-images.githubusercontent.com/75629402/102102554-3eeaf200-3e2c-11eb-894c-504e75b3274a.png)

![USBasp_build2](https://user-images.githubusercontent.com/75629402/102102602-4d390e00-3e2c-11eb-8b53-cb151460a55f.png)

## Software
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; You can find the software to be flashed into our shiny new USBasp on Fischl's [website](https://www.fischl.de/usbasp/usbasp.2011-05-28.tar.gz).

## Closing notes
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; !!!Happy tinkering!!! :D

## References
- [Wikipedia](https://en.wikipedia.org/wiki/Time#Measurement)
- [Network Time Protocol](http://support.ntp.org/bin/view/Main/WebHome)