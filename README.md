# RTB_E10
[![Real-time Bus (RTB)](https://img.shields.io/badge/RTB_Project-FF6699)](https://www.rtb4dcc.de)
[![Kicad_Libs](https://img.shields.io/badge/Kicad_Libs-29C7FF)](https://github.com/git4dcc/RTB_SamacSys)
[![Apache License 2.0](https://img.shields.io/badge/license-Apache%20License%202.0-lightgray)](https://www.apache.org/licenses/LICENSE-2.0)

This E10 module implements a 16 channel WS2811 emulator with compatible bus timing. The E10 may be cascaded with regular WS28xx chips. The number of LEDs is auto detected/configured (0-16). Optionally, the common LED voltage can be controlled over the bus as well.

<details>
<summary>See also</summary>

- [RTB_E13](/../../../../git4dcc/RTB_E13)
- [RTB_E15](/../../../../git4dcc/RTB_E15)

</details>

<details>
<summary>User Guides</summary>

- [User Guide - DE](https://rtb4dcc.de/ws2811_guide_de/)
- User Guide - EN

</details>

<img src=supplemental/images/E10_main.jpg>

The decoder has the following features,
- **Protocol**
  - WS2811 compatible timing
- **LED ports**
  - 0..16 channel output (auto detecting)
  - Software selectable common LED voltage (0 .. 5V)
  - Push/Pull output and can drive common anode or cathode
  - 256 step PWM (240Hz)
  - gamma correction (optional)
- firmware update via V24 debug interface

# Hardware
My current PCB layout uses SMD footprints with 0.5mm pitch and 0603 parts. Reflow soldering is my recommendation, but with some experience handsoldering is also possible.

The hardware allows either push or pull operation. Currently only the pull operation is implemented in the firmware. Should a usecase for push-operation arise, the firmware could be adapted.

<img src=supplemental/images/E10_top_connect.jpg>

## PCB

- 2-layer PCB, FR4, 1.6mm
- CPU: AVR64DA32
- BUS: WS28xx
- LED: Push/Pull

## Kicad
[Schematic](doc/E10_schematic.pdf) | [Layout](doc/E10_layout.pdf) | [Gerber](gerber)

<details>
<summary>Dependency</summary>
<br>

:yellow_circle: Requires my Kicad project library [RTB_SamacSys](/../../../../git4dcc/RTB_SamacSys) in the same directory tree.

</details>

## Firmware
Filename structure: { **pcb** }{ **code** }{ **version** }.hex

Example: **E10F0001**.hex

|   | Description |
| --- | --- |
| **pcb** | Name of matching hardware (**E10**) |
| **code** | Type of code contained (**R**=rom, **B**=bootloader, **F**=flash, **U**=bld update, **P**=UPDI factory code) |
| **version** | Release version (**####**) |

[Firmware files](firmware)

## UPDI
The fuse settings as well as the P-code (E10Pxxxx.hex) has to be installed by using UPDI.<br>

<details>
<summary>Details</summary>

<img src=supplemental/images/E10_updi.jpg>

| Fuse Setting | P-Code Install |
| --- | --- |
|<img src=supplemental/images/E10_updi_fuses.jpg width=500>|<img src=supplemental/images/E10_updi_memory.jpg width=500>|

</details>

## Debug Interface
Subsequent code updates can be done via the built-in serial debug interface.<br>

<details>
<summary>Details</summary>

- connect the serial cable **(1Mb, 8N1, RTS/CTS)**
- press 'break' within the VT100 terminal to bump the module to console prompt
- upload the firmware file (E10Fxxxx.hex)
- for more details, refer to the 'User Guide'

  <img src=supplemental/images/E10_debugIF.jpg>
</details>

# Software
The LED common voltage must be sent as the first byte (virtual LED) over the bus followed by the intensity values for the individual LEDs.

```
Byte order:     {voltage} {led_0} ... {led_n}      //'n' being the number of configured LEDs
Led Voltage:    5V * {voltage} / 255
```

# Images
<img src=supplemental/images/E10_samples.jpg width=260> <img src=supplemental/images/E10_usecase.jpg width=265>

This project is intended for hobby use only and is distributed in accordance with the Apache License 2.0 agreement.
