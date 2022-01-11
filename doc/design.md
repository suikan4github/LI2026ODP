# Design 
This file explains how and why of the circuit design of LI2026ODP.

## Information for software
### VFD signal mapping
The VFD driver MAX6921 is a shift register for the serial-parallel converter. This register's parallel pins are connected to the digit drive grid and the segments of VFD. 

Bit | Signal
----|-------------
00  | Seg b
01  | Seg f
02  | Seg a
03  | D0
04  | D1
05  | D2
06  | D3
07  | D4
08  | D5
09  | D6
10  | D7
11  | D8
12  | N.A.
13  | N.A.
14  | N.A.
15  | Seg DP
16  | Seg d
17  | Seg c
18  | Seg e
19  | Seg g

Where D0..D8 represents the digit 1 to 9 ( from right to left). 

The seg a to seg g and seg DP are illustrated as below. 

![segments](https://user-images.githubusercontent.com/26223147/140596731-c114274a-8d59-4f17-8a82-a02779a39ff3.png)

The specific segment is "ON" when both its digit signal and segment signal are "H" ( 0 in logic ).

### Key Matrix
See [Wiki Page](https://github.com/suikan4github/LI2026ODP/wiki/KeyMatrix) as details of key matrix. 

### MCU port mapping. 
Pin  | Name | Signal
----:|------|------
 1   | PB9  | KEY0 
 6   | PF2  | NRST
 7   | PA0  | KEY1
 8   | PA1  | KEY2
11   | PA4  | BLANK
12   | PA5  | SPI1 SCK
13   | PA6  | LOAD
14   | PA7  | SPI1 MOSI
24   | PA13 | SWDIO
25   | PA14 | SWCLK
30   | PB6  | LPUART2 TX
31   | PB7  | LPUART2 RX

SPI1 must be set as :
- CPOL = 0
- CPHA = 0

## Hardware design
### Pin compatibility
The LI2026ODP is pin compatibility with the LI2026A. So, it is 28pin 300mil DIP shape. The pin assignment is shown in the [Wiki page](https://github.com/suikan4github/LI2026ODP/wiki/LI2026A).

In addition to the 28pins, it has 5 pin debug port for the uploading program and on board debug. The debug port has not only SWD signals but also UART TX signal. 

The debug port is 2.54mm pitch. The signal assignment is :
Pin | Description
----|-------------
1   | SWCLK
2   | GND
2   | SWDIO
3   | NRST
4   | LPUART2 TX

### Power supply
The the SHARP EL-210 which uses LI2026A has internal DC-DC coveter to supply the VFD segment / grid drive voltage and all other bias voltages. In the other words, LI2026A doesn't have any internal power supply which provide any voltage source to the board. 

The original LI2026A seems to be PMOS design. That mean, it works with the minus voltage to the GND. See the [Wiki page](https://github.com/suikan4github/LI2026ODP/wiki/LI2026A). for details. Followings are the pin assignment of the LI2026A regarding the power supply 
Pin | Description
----|-------------
3   | -9V
4   | -27V
5   | -9V
14  | GND
15  | -7V
16  | -9V
27  | -9V

We use only pin 4 and 14. And renamed in the LI2026ODP as followings : 

Pin | Description
----|-------------
4   | GND
14  | 27V

Thus, int is positive voltage circuit internally. 

### Voltage regulator circuit
Because the high voltage input, regular voltage regulator may be destroyed. The LM2936MM-3.3 can work with such the high voltage. 

The datasheet has specific request to the output capacitor of LM2936MM-3.3. The capacitance and ESR is bit critical. Refer the datasheet for details. 

### SWD pin protection
ARM suggest to place the pull up registers to protect the SWD pins from the static discharge. 

We don't have these registers. Alternatively, the programmer MUST follow the procedure in the reference manual of the STM32G0B1. The RM recommend to set the SWD pins as GPIN and pull up / down by software setting. Refere the reference manual. 

### NRST pin protection
The STM32G0B1 has internal pull up register for the NRST signal. So, we don't need the external register. 

The datasheet recommend to have the 0.1uF capacitor as parallel to the NRST input, to protect from the RF interference. 

### Key matrix diode
The EL-210 calculator shares the segment drive signal with the key matrix scan signal. But this low cost calculator doesn't have the protection diode on the board. Perhaps, it is internally implemented inside LI2026A. 

Thus, also LI2026ODP has protection diodes internally. These diodes protect the MAX6921 from the "double push" operation which causes the segment drivers short circuit.  The diode must work with the revers bias as least  27V. 

