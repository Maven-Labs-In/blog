---
title: 'The Matrix'
date: 2020-12-20 10:41:30 +/-0530
categories: [keys, keypad, atmega328p]
tags: [matrix keypad]     # TAG names should always be lowercase
---

>“Unfortunately, no one can be told what The Matrix is. You'll have to see it for yourself.”
― Morpheus

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; for this blog we could change it to something more meaningful like below :)
>“Unfortunately, no one can be told what The Matrix Keyboard is. You'll have to Do it for yourself, and learn.”

## Sensing Inputs
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; MCU interacts with real world using sensors and actuators. In this blog, lets consider the sensors. There are different types of sensors that can be sensed by MCU to measure different parameters in real world for control purposes. 

## The 'Humble Switch'
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; One of the simplest input sensors we can read from the MCU is the 'digital switch'. 

## Switch Configurations
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; There are different configurations possible to connect switch to your MCU. Which variant to choose depends on the particular application or use case.

### Switch with Pull-Up resistor
![pull-up-switch](https://user-images.githubusercontent.com/75629402/102707478-95788600-429b-11eb-866a-8012506c15f6.png)
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
