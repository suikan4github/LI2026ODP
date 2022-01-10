# LI2026ODP
Over Drive Processor replacement for the SHARP LI2026A single chip calculator

# Description
The LI2026ODP is an over drive replacement of the SHARP LI2026A "calculator-on-a-chip" LSI.

Without any additional circuit board modification, LI2026ODP allow you to control the all keys and all segments of VFD. You can find a firmware at the [rpn2026 project](https://github.com/suikan4github/rpn2026).

The on board STM32G0B1KET6 micro computer 
has enough speed and memory to run a complex
firmware of a pocket calculator.

In addition to the design, followings are provided in this repository :
- [Schematic diagram](doc/LI2026ODP.pdf). 
- [BOM](doc/LI2026ODP.csv)

# Photos

# Requirement
This PCB is designed with :
- KiCAD 5.1.12

# Usage
To use LI2026ODP, simply replace the original LI2026A with the ODP board. Any other modification of the circuit board is not required. 

The SWD and UART port are provided to program and debug the on board microprocessor in the J2 port :

Pin | Description
----|-------------
1   | SWCLK
2   | GND
2   | SWDIO
3   | NRST
4   | LPUART2 TX


# License
This PCB design is distributed by CC0. See [LICENSE](LICENSE) file. 