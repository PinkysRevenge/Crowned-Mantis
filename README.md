# Crowned-Mantis
## A CPAP toolhead for Voron Trident and v2.4

![Crowned-Mantis](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/CrownedMantis_Front.jpg)

This project is still in early development. Use at your own risk and please provide feedback. 

Discord: Pinky’s Revenge#0561

Credits - this toolhead uses:
 - A modified duct from the CrownCooler toolhead by SneakyTreeSnake (https://github.com/sneakytreesnake/CrownCooler)
 - The X Carriage and magprobe of the Mantis Toolhead by Long (https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/Long/Mantis_Dual_5015)
 - CPAP sourcing and setup instructions by Chirpy (https://github.com/chirpy2605/voron/tree/main/V0/StovePipe)

## BOM

- Rapido HF Hotend (if you'd like a version for another hotend, let me know)
- Sherpa Mini Extruder (I plan to make a version for Orbiter/VZ-Hextrudor soon) 
- 3010 hot end fan
- CPAP fan, controller, and 15mm ID CPAP hose (see below for more details on CPAP setup)
- Mantis carriage and magprobe setup
- 5x 3mm heat set inserts (not including others needed for the Mantis carriage and Sherpa Mini
- 3x 2mm heat set inserts (if you want a version that uses 3mm let me know)
- Various 3mm and 2mm screws

## Parts to Print
1. Mantis carriage and magprobe parts according to the github here: https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/Long/Mantis_Dual_5015
2. Sherpa Mini extruder parts according to the gitbhub here: https://github.com/Annex-Engineering/Sherpa_Mini-Extruder
3. Crowned-Mantis_HE_Mount. Does require supports (touching build plate only), otherwise use standard Voron print specs.
4. Crowned-Mantis_Duct. Recommend 0.1mm layer height. Supports (touching build plate only) can be helpful for the outlets if your bridging isn't great, but they aren't necessary.
5. Optional: Crowned-Mantis_Sherpa_Housing_Front replaces the normal Sherpa Mini Housing_Front for added stiffness
6. Optional: Simon Vez has a great CPAP fan mount and outlet adapter here: https://github.com/VzBoT3D/Vz-Printhead-CNC/tree/main/CAD/CPAP%20turbo%20cooling%20systemhttps://github.com/VzBoT3D/Vz-Printhead-CNC/tree/main/CAD/CPAP%20turbo%20cooling%20system

## Toolhead Assembly

1. Build the Mantis carriage and magprobe according to the github
2. Build the Sherpa Mini extruder according to the gitbhub
3. Install 5x 3mm heat inserts into the HE mount to hold the duct and the extruder

![Front_View](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/Hotend%20Mount%20Rapido%20Sherpa%20Mini%20Front%202.jpg)

![Top_View](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/Hotend%20Mount%20Rapido%20Sherpa%20Mini%20Top%202.jpg)

4. Install 3x 2mm heat inserts into the HE mount to hold the fan

![Fan_Mounts](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/fan%20mounts.jpg)

6. Screw the hot end to the HE mount
7. Screw the HE mount to the Mantis carriage
8. Screw the Sherpa Mini, HE fan and duct to the HE mount

![Side_View](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/Crowned-Mantis%20Side%202.jpg)

![Crowned-Mantis](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/Crowned-Mantis%202.jpg)

## CPAP Sourcing and Setup

CPAP fans not only have a high CFM, they generally also have very high static pressure. This allows air to be delivered to the hotend with greater force than traditional blower fans.

This mod uses the WS7040-24-V200 from Wonsmart which needs to be sourced with a supported driver board. This allows the fan to run at 24v, but the fans PWM is controlled by a 5v data logic controller.

![ws7040](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/ws7040.jpg)
![WS2403DY01V04](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/WS2403DY01V04.jpg)

It can also be controlled by a 3.3V  data logic controller such as the printers MCU, but you will usually be limited to ~60% of the fans potential. To increase this you can use a logic level converter to translate the 3.3v data signal to 5v.

This can be achieved in various ways, e.g. using an MCU's neopixel port since they must be driven by 5v data logic. Alternatively, a dedicated logic level converter can be connected to a spare port on the raspberry PI, such as the [Sparkfun BOB-12009](https://www.sparkfun.com/products/12009) which works very well.

The main disadvantage of only using a logic level shifter is that the fan will run at 100% when power is first fed to the PI pin as it floats before being explicitly set. This can be countered by powering the fan itself through a spare heater port on the MCU which you may have free and using it as an enable pin in klipper.

As a simple solution that addresses all of these requirements and concerns, I'm using the [Klipper Expander](https://github.com/VoronDesign/Voron-Hardware/tree/master/Klipper_Expander). The advantages with this solution is that you can control the fan through the Neopixel pin (only 1 wire needed) and explicitly power the fan on/off through one of the 24v MOSFETs. This is important because traditional 24v MCU fan headers won't usually support the 2.8A that this fan can pull at 100% load which can lead to a damaged MCU. 

This mod works optimally with a 15mm ID CPAP hose such as [this one](https://www.amazon.co.uk/gp/product/B07TCC42WT) that I have used. They can also be found on Aliexpress.

The simplest way to purchase the WS7040 fan is through [Mellow on Aliexpress](https://www.aliexpress.com/item/1005004729010078.html) as this comes together with the required driver board and at the correct voltage. [Trianglelab](https://www.aliexpress.com/item/1005003822117604.html) also have a complete kit, however the included CPAP hose is not 15mm ID and won't work directly with this implementation. There are many other sellers for the WS7040 at widely varying prices. Just be sure to get the correct model (WS7040-24-V200) together with the associated [driver board](https://www.wonsmart.com.cn/ws2403-15655936300127195.html).

## Wiring:

![WS2403DY01V04](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/WS2403DY01V04.png)
![Klipper Expander](https://github.com/PinkysRevenge/Crowned-Mantis/blob/main/Images/STM32_Klipper_Expander.png)

Run a wire from the second from the bottom pin (red circle) on the driver board to the centre neopixel pin (red circle) on the klipper expander.

Connect the 24v connector (blue circle) on the driver board to one of the MOSFET ports (blue circle) on the Klipper Expander, paying attention to the polarity.

Connect the large connector from the fan to the driver board (yellow circle).

Connect your 24v supply (e.g. from the printer PSU) to the Klipper Expander (yellow circle). See the [Klipper Expander](https://github.com/VoronDesign/Voron-Hardware/tree/master/Klipper_Expander/Documentation) documentation on how to connect and configure it to the printer.

Buy me a coffee
https://www.buymeacoffee.com/PinkysRevenge
