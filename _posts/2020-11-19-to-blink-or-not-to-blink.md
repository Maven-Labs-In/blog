---
title: To blink or not to blink :)
date: 2020-11-19 11:30:45 +/-0530
categories: [Microcontroller, avr]
tags: [blinky]     # TAG names should always be lowercase
---

## HW Platform to be used
### Microchip(formerly ATMEL) 328P Controller

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This post is yet another blinky program on popular Microchip ATMEGA 328p micro controller. We will be using  [Arduino nano](https://store.arduino.cc/arduino-nano) board for our experiment. Since the HW is readily available we can concentrate on writing our first program for blinking the LED!

### Software development environment for blinky
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We will be using Bare metal programming technic to program the microcontroller. we will need below tools for the development.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;I will be using __Zorin OS__, which is a linux distribution based on ubuntu platform. You can download a copy of this linux flavour [here](https://zorinos.com/) for free :blush:
Instructions given in this blog should work on most of the ubuntu flavoured OSs.

1. Compiler
2. Programmer software
3. Your favourite code editor!

### Software Tools Setup
#### AVR GCC Compiler 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We will be using AVR GCC compiler which is based on a open source compiler developed by free software foundation(FSF). Full form of GCC is __GNU Compiler Collection__. GCC compiler can used for many languages like, C, C++ etc.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Goto Microchip website and [download](https://www.microchip.com/mplab/avr-support/avr-and-arm-toolchains-c-compilers) latest version of AVR GCC compiler for linux. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If you need to check whether your linux OS is 32bit or 64bit you can execute below command.

> $ uname -m
>    <br />x86_64


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;So, i will be downloading 64bit avr-gcc toolchain [AVR 8-bit Toolchain 3.6.2 - Linux 64-bit](https://www.microchip.com/mymicrochip/filehandler.aspx?ddocname=en607660), which is latest(and greatest), at the time of this writing.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Let's install the downloaded setup file 'avr8-gnu-toolchain-3.6.2.1778-linux.any.x86_64.tar.gz' in linux now. Below set of commands will install the avr-gcc compiler on your PC.

> tar -xzf avr8-gnu-toolchain-3.6.2.1778-linux.any.x86_64.tar.gz

Update the PATH variable to AVR-GCC tool folder
> $ nano ~/.bashrc

**!!! Beware, you will break the bash command execution if you overwrite PATH variable!!!** 

add at the end of the file to update PATH variable with avr-gcc compiler path 
> export PATH=$PATH:/home/tools/avr8-gnu-toolchain-linux_x86_64/bin/

A quick check for avr-gcc version can now be done.
> $ avr-gcc --version
>    <br />avr-gcc (AVR_8_bit_GNU_Toolchain_3.6.2_1778) 5.4.0
>    <br />Copyright (C) 2015 Free Software Foundation, Inc.
>    <br />This is free software; see the source for copying conditions.  There is NO
>    <br />warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Rejoice, your avr-gcc compiler set up is now complete :)

#### AVR Libc & Binutils

Unfortunately, we can not use the compiler installation we did just now! We need to still install [avr-libc](https://www.nongnu.org/avr-libc/) & [avr-binutils](https://www.archlinux.org/packages/community/x86_64/avr-binutils/)

avr libc provides c runtime libraries for AVR controllers. Binutils is binary utilities to handle AVR binary files.

> $ sudo apt-get install avr-libc binutils-avr

#### AVR Programmer utility: AVRDUDE
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We need a programmer software to burn our compiled program into AVR chips. We will be using avrdude software for this. Installation is fairly simple and straight forward. you can install avr dude using below command in terminal.
> $ sudo apt-get install avrdude

#### Your favourite code editor!
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Last but not least, we need a editor program to write our programs. i will be using **visual studio code**. Code is a free SW editor developed by Microsoft with open source license. Installation procedure is given below on ubuntu.

* goto --> Code download [link](https://code.visualstudio.com/Download)
* Download __Code.deb__ package for ubuntu. you also have installer files for other OS's on the same link.
* Install MS code with below command
  * > $ sudo dpkg -i code_1.51.1-1605051630_amd64.deb
* MS code is installed now. Close the terminal.

## Let's write some code, Finally!!!
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Hold your horses, we are not there yet :P Before writing any programs in embedded system, we need to be aware of HW connections we are suppose to sense/control in programs. In our case we need to blink an LED. Let's take a closer look at the [schematics](https://content.arduino.cc/assets/NanoV3.3_sch.pdf) to ascertain which port the LED is connected.
As per the below schematic snippet, it is clear that LED is connected to port PB5. 

![blinky_led_port](https://user-images.githubusercontent.com/75629402/101634671-52135180-3a29-11eb-9cac-2703b8523f42.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We have all the necessary information and required tools to control the LED now, It's time to *BLINK* the LED :)

## blinky C code
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Code snippet below to blink the LED connected to PB5 pin in arduino nano.

    #include <avr/io.h>
    #include <util/delay.h>
    
    int main(void) 
    {
       DDRB |= 1 << PB5;          /* set LED pin PB5 to output */
       while(1) 
       {
         PORTB &= ~(1 << PB5);    /* LED off */
         _delay_ms(900);          /* delay 900 ms */
         PORTB  |= 1 << PB5;      /* LED on */
         _delay_ms(100);          /* delay 100 ms */
       }
       return 0;
    }

## blinky project make file
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Makefile sample below to build/clean/program the blinky project.

    #==== Main Options =============================================================
    
    MCU = atmega328p
    F_CPU = 16000000
    TARGET = blinky
    
    
    #==== Compile Options ==========================================================
    
    CFLAGS = -mmcu=$(MCU) -I.
    CFLAGS += -DF_CPU=$(F_CPU)UL
    CFLAGS += -Os
    #CFLAGS += -mint8
    #CFLAGS += -mshort-calls
    CFLAGS += -funsigned-char
    CFLAGS += -funsigned-bitfields
    CFLAGS += -fpack-struct
    CFLAGS += -fshort-enums
    #CFLAGS += -fno-unit-at-a-time
    CFLAGS += -Wall
    CFLAGS += -Wstrict-prototypes
    CFLAGS += -Wundef
    #CFLAGS += -Wunreachable-code
    #CFLAGS += -Wsign-compare
    CFLAGS += -std=gnu99
    
    #==== Programming Options (avrdude) ============================================
    
    AVRDUDE_PROGRAMMER = arduino
    AVRDUDE_PORT = /dev/ttyUSB0
    AVRDUDE_BAUD = 9600
    AVRDUDE_FLAGS = -p $(MCU) -c $(AVRDUDE_PROGRAMMER) -P $(AVRDUDE_PORT)
    
    #===============================================================================
    #==== Targets ==================================================================
    
    AVR_GCC_PATH = /usr/bin
    CC = $(AVR_GCC_PATH)/avr-gcc
    OBJCOPY = $(AVR_GCC_PATH)/avr-objcopy
    SIZE = $(AVR_GCC_PATH)/avr-size
    NM = $(AVR_GCC_PATH)/avr-nm
    AVRDUDE = avrdude
    REMOVE = rm -f
    
    all: build
    
    build:
    	$(CC) -c $(CFLAGS) $(TARGET).c -o $(TARGET).o
    	$(CC) $(CFLAGS) $(TARGET).o --output $(TARGET).elf
    	$(OBJCOPY) -O ihex -j .text -j .data $(TARGET).elf $(TARGET).hex
    	$(SIZE) -C --mcu=$(MCU) $(TARGET).elf
    	#$(NM) $(TARGET).elf
    
    program:
    	$(AVRDUDE) $(AVRDUDE_FLAGS) $(AVRDUDE_WRITE_FLASH) -U flash:w:$(TARGET).hex
    
    clean:
    	$(REMOVE) "$(TARGET).hex"
    	$(REMOVE) "$(TARGET).elf"
    	$(REMOVE) "$(TARGET).o"

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The blinky project can be downloaded from our Github [repository](https://github.com/Maven-Labs-In/blinky.git) as always :)

## Fruit of our hard work :P
**Behold the blinky...... Yay!!!**

![blinky](https://user-images.githubusercontent.com/75629402/101639032-1bd8d080-3a2f-11eb-94ac-2319da239b3c.gif)