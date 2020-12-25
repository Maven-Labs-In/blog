---
title: Hello World
date: 2020-12-20 16:02:15 +/-0530
categories: [lcd, 16x2, 20x4, atmega328p]
tags: [16x2-lcd]     # TAG names should always be lowercase
---

![20x4_LCD](https://user-images.githubusercontent.com/75629402/103137053-e6d2ac00-46c5-11eb-88ae-e526fc3d0d58.png)

## Background
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "Hello World" is a C program written by Dennis Ritchie(The Inventor of C programming Language) in his book __The C Programming Language__ to showcase the capabilities of the C language to print a string "Hello World". In this blog we will tweak this famous program a bit and display "Hello World" message on a 20x4 LCD display.

## The IO Problem with MCUs!!!
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; As explained in [keypad](../_site/posts/the-matrix) article, each new input or output addition to MCU requires usage of an IO pin. If we need to display numbers 0 to 9 then we need to interface a __seven segment display__! including the dot LED, we need a total of 8 IO pins to control a single digit display from MCU. imagine if we need to display a sentence as output in our application. That would require more than 300 IO pins from MCU. This is not a feasible design as MCU's will have very few IO pins. 

## HD44870 based LCD display
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; This is where __HD44870__ based character displays come to the rescue. They have a built in LCD controller which can control a variaty of LCD configurations as listed below. Datasheet for HD44780 controller is availble online [here](https://www.sparkfun.com/datasheets/LCD/HD44780.pdf).
- 16x1
- 16x2
- 20x4


## LCD interface pins
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; HD44870 LCD controller based displays have 16 IO pins as listed below.

| PIN | LCD | Nano |  Meaning            |   Connection                |
|-----|------|------|---------------------|-----------------------------|
| 1   | VSS  | VSS  | Ground pin          |   GND                       |
| 2   | VDD  | VDD  | Supply pin          |   VCC                       |
| 3   | VEE  |  -   | Contrast set pin    |   0.18V                     |
| 4   | RS   | D8   | Register select pin |   HIGH/LOW based on need.   |
| 5   | R/W  | GND  | Read/Write pin      |   GND                       |
| 6   | E    | D9   | Execute pin         |   /Execute control line     |
| 7   | D0   | D0   | LSB data pin        |   Parallel data d1 pin LSB  |
| 8   | D1   | D1   | data pin            |   Data D2 pin               |
| 9   | D2   | D2   | data pin            |   Data D3 pin               |
| 10  | D3   | D3   | data pin            |   Data D4 pin               |
| 11  | D4   | D4   | data pin            |   Data D5 pin               |
| 12  | D5   | D5   | data pin            |   Data D6 pin               |
| 13  | D6   | D6   | data pin            |   Data D7 pin               |
| 14  | D7   | D7   | MSB data pin        |   Parallel data d7 pin MSB  |
| 15  | LED+ |  -   | data pin            |   4.3V                      |
| 16  | LED- |  -   | MSB data pin        |   GND                       |

## HD44870 Interface v1: 8-bit mode.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Pin configurations for LCD iunterfacing to atmega328p is shown above. 
![8-bit-LCD-Interface](https://user-images.githubusercontent.com/75629402/103139790-227a6f80-46e0-11eb-87ea-849af11adb60.png)



## HD44870 Interface v2: 4-bit mode.
![4-bit-LCD-Interface](https://user-images.githubusercontent.com/75629402/103139843-e0056280-46e0-11eb-89cd-c0938b128a5c.png)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Here, a resistor is connected to switch and pulled up to supply. When the switch status is, 
 - Closed - Output is at Low-State. 
 - Open  &nbsp;&nbsp;  - Output is at High-State. 
### Switch with Pull-Down resistor
![pull-down-switch](https://user-images.githubusercontent.com/75629402/102707484-a75a2900-429b-11eb-8436-1226454dff06.png)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; In case of switch with pull-down resistor configuration the output level of circuit is inverted as below.
 - Closed - Output is at High-State. 
 - Open  &nbsp;&nbsp;  - Output is at Low-State. 
### Matrix Keypad
![4x4_matrix_keypad](https://user-images.githubusercontent.com/75629402/102712218-aa671080-42bf-11eb-89d5-a59efe9f8a0d.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If the applicaton has few switches, one of the above configurations will suffice. Let's consider if there are 16 switches needed for an application then, 16 IO pins are needed from MCU! Due to limited number of IO pins available, we may not be allocate 16 IO pins for keyboard. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; One way to reduce the IO pin requirements is to arrange the keys in matrix keypad configuration. One such 4x4 configuration is shown above. Here, we use 8 IO pins and detect upto 16 switches. There are different configurations possible like, 3x3, 3x4 or 5x5 etc ....

## Pro's and Con's of different switch configurations
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Ofcourse there are both pro's and con's for each method as listed below
1. Discrete(pull-up/pull-down) configuration:
   - Advantages 
     - Simple circuit.
     - Simple SW.
   - Dis-advantages
     - More switches need more IO pins.

2. Matrix configurations
   - Advantages
     - Less number of IO pins needed.
     - Can be adapted to different configurations as needed.
   - Dis-advatages
     - Complex circuit for keypad.
     - SW complexity is more to decode a switch.

## Software
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Famous word's from Linus Torvalds, the father of Linux operating system, comes to my mind.
>"Talk is Cheap, Show me the code!"
-- Linus Torvalds

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Let's evaluate how the code can be constructed to decode the key pressed in matrix configuration. Row's and column's of 4x4 matrix keypad is connected to MCU as per below list.

### MCU port &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp;Keypad connections
   - Row_0 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D0 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD0
   - Row_1 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D1 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD1
   - Row_2 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D2 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD2
   - Row_3 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D3 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD3
   - Col_3&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D4 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD4
   - Col_2&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D5 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD5
   - Col_1&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; D6 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD6
   - Col_0&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;--> &nbsp;&nbsp;&nbsp;D7 &nbsp;&nbsp;&nbsp;-->&nbsp;&nbsp;&nbsp; PD7

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; We are using port D of atmega328p microcontroller for our interfacing of 4x4 keypad. Lower 4 bits(PD0-PD3) are used for Row connections and Upper 4 bits(PD4-PD7) are used for Column connections of keypad. This particular connection scheme makes the SW design a bit easier :)

### Software design
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; We will be using bitwise operators to scan the keypad and decode the key pressed in keypad.

    
    #define ROW_COUNT         (4)
    #define COL_COUNT         (4)
    #define ROW_PORT          (PORTD)
    #define COL_PORT          (PORTD) 
    #define ROW_MASK          (0b00000001)
    #define COL_MASK          (0b10000000)
    #define NO_KEY_PRESSED    (255)

    uint8 key_scan(void)
    {
        uint8 col_mask = COL_MASK;
        uint8 row_mask;
        uint8 key_pressed = 0;

        for(row_counter = 0; row_counter < ROW_COUNT; row_counter++)
        {
            row_mask = ROW_MASK;
            for(col_counter = 0; col_counter < COL_COUNT; col_counter++)
            {
                ROW_PORT = row_mask;
                if(COL_PORT & col_mask)
                {
                    return (key_pressed);
                }
                else
                {
                    key_pressed++;
                }
                col_mask = col_mask >> 1;
            }
            row_mask = row_mask << 1;
        }
        return (NO_KEY_PRESSED);
    }
    
## Switch Debouncing
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Switch is a mechanical device and when a switch is pressed the switch plate gets closed. The switch closure doesnt produce a step response as we expect in an ideal switch. There are some spikes which are produces whenever a key is pressed or released. These spikes needs to filtered(debounced) to get proper key readings. __key_scan()__ function can be called once in a ms and a debounce of 10 to 20 times counts can be done to rule out errors due to mechanical debounce.

## Build images
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 

## Closing notes
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; !!!Happy tinkering!!! :D

## References
