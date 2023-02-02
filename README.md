# Crowned-Mantis  A CPAP toolhead for Voron Trident and v2.4

This toolhead uses a modified duct from the CrownCooler toolhead from SneakyTreeSnake (https://github.com/sneakytreesnake/CrownCooler) that mounts to the X Carriage of the Mantis 5015 Toolhead by Long (https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/Long/Mantis_Dual_5015) to create a light and stiff setup for CPAP on Voron 2.4 and Trident printers.

(Instructions below courtesy of Chirpy, checkout his great CPAP mod for the v0.1 here: https://github.com/chirpy2605/voron/tree/main/V0/StovePipe)

CPAP fans not only have a high CFM, they generally also have very high static pressure. This allows air to be delivered to the hotend with greater force than traditional blower fans.

This mod uses the WS7040-24-V200 from Wonsmart which needs to be sourced with a supported driver board. This allows the fan to run at 24v, but the fans PWM is controlled by a 5v data logic controller.

![ws7040](images/ws7040.jpg)
![WS2403DY01V04](images/WS2403DY01V04.jpg)

It can also be controlled by a 3.3V  data logic controller such as the printers MCU, but you will usually be limited to ~60% of the fans potential. To increase this you can use a logic level converter to translate the 3.3v data signal to 5v.

This can be achieved in various ways, e.g. using an MCU's neopixel port since they must be driven by 5v data logic. Alternatively, a dedicated logic level converter can be connected to a spare port on the raspberry PI, such as the [Sparkfun BOB-12009](https://www.sparkfun.com/products/12009) which works very well.

The main disadvantage of only using a logic level shifter is that the fan will run at 100% when power is first fed to the PI pin as it floats before being explicitly set. This can be countered by powering the fan itself through a spare heater port on the MCU which you may have free and using it as an enable pin in klipper.

As a simple solution that addresses all of these requirements and concerns, I'm using the [Klipper Expander](https://github.com/VoronDesign/Voron-Hardware/tree/master/Klipper_Expander). The advantages with this solution is that you can control the fan through the Neopixel pin (only 1 wire needed) and explicitly power the fan on/off through one of the 24v MOSFETs. This is important because traditional 24v MCU fan headers won't usually support the 2.8A that this fan can pull at 100% load which can lead to a damaged MCU. 

This mod works optimally with a 15mm ID CPAP hose such as [this one](https://www.amazon.co.uk/gp/product/B07TCC42WT) that I have used. They can also be found on Aliexpress.

The simplest way to purchase the WS7040 fan is through [Mellow on Aliexpress](https://www.aliexpress.com/item/1005004729010078.html) as this comes together with the required driver board and at the correct voltage. [Trianglelab](https://www.aliexpress.com/item/1005003822117604.html) also have a complete kit, however the included CPAP hose is not 15mm ID and won't work directly with this implementation. There are many other sellers for the WS7040 at widely varying prices. Just be sure to get the correct model (WS7040-24-V200) together with the associated [driver board](https://www.wonsmart.com.cn/ws2403-15655936300127195.html).
