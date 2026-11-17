# CyberLono

![Uploading image.png…]()

_Hello everyone!_

CyberLono is a small cyberdeck that uses a Pi Zero 2w, a CardKB keypad by M5STACK, a 3.5" LCD touchscreen. a MAX98357A for a 8Ohm 1W Speaker (34.8mm*11.2mm*6.0mm) and a IP5310 charging module for the lithium battery. 

For the lithium battery, I will be using a 3.7V single cell Li-Po battery 3500mah that will be enough for maybe 4+ hours

I use four 5mmx2mm neodynium magnet so it will not be loose when closed

## CAD

https://cad.onshape.com/documents/96cb9afee00ff4fbaaa8108e/w/c905764df47b4c9fb6a24cde/e/d96e04691eea43e5f5dad68a?renderMode=0&uiState=6afb07153b5dcb2cf40f343c

## Wiring

| Subsystem | From Pin | To Pin |
| :--- | :--- | :--- | :--- |
| **Power** | 3.7V LiPo Battery (`+`) | IP5306 Module (`B+`) |
| **Power** | 3.7V LiPo Battery (`-`) | IP5306 Module (`B-`) |
| **Power** | IP5306 Module (`5V`) | Pi Zero 2 W (`Pin 2` or `Pin 4` - 5V) |
| **Power** | IP5306 Module (`IN-`) | Pi Zero 2 W (`Pin 6` - GND) |
| **Audio** | Pi Zero 2 W (`Pin 2` or `Pin 4` - 5V) | MAX98357A (`VIN`) |
| **Audio** | Pi Zero 2 W (`Pin 9` - GND) | MAX98357A (`GND`) |
| **Audio** | Pi Zero 2 W (`Pin 12` / `GPIO 18`) | MAX98357A (`BCLK`) |
| **Audio** | Pi Zero 2 W (`Pin 35` / `GPIO 19`) | MAX98357A (`LRC`) |
| **Audio** | Pi Zero 2 W (`Pin 40` / `GPIO 21`) | MAX98357A (`DIN`) |
| **Audio** | MAX98357A (`+` and `-`) | CES-281043 Speaker (`Red` & `Black`) |
| **Keyboard** | Pi Zero 2 W (`Pin 1` - 3.3V) | CardKB (`3V3` / `VCC`) |
| **Keyboard** | Pi Zero 2 W (`Pin 14` - GND) | CardKB (`GND`) |
| **Keyboard** | Pi Zero 2 W (`Pin 3` / `GPIO 2`) | CardKB (`SDA`) |
| **Keyboard** | Pi Zero 2 W (`Pin 5` / `GPIO 3`) | CardKB (`SCL`) |

Note : CardKB uses a 4-pin HY2.0-4P connector. So use a 4-pin HY2.0-4P to dupont wire

## BOM 

| Componets | Qty | Price (USD) | Buy Link |
| :--- | :--- | :--- | :--- |
| Raspberry Pi Zero 2 W | 1 | $35 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-Raspberry-Pi-Zero-2-W.html?spm=a2g0o.productlist.search.0) |
| 3.5" SPI Touchscreen Display | 1 | $15| [AliExpress Search](https://www.aliexpress.com/w/wholesale-3.5-inch-Raspberry-Pi-display.html?spm=a2g0o.productlist.search.0) |
| M5Stack CardKB (I2C Mini QWERTY Keyboard) | 1 | $12.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-M5Stack-CardKB.html?spm=a2g0o.productlist.search.0) |
| MAX98357A I2S 3W Class D Amp Breakout | 1 | $4.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-MAX98357A-I2S-amplifier.html?spm=a2g0o.productlist.search.0) |
| Ghxamp Mini Speaker Cavity Loudspeaker 8Ohm 1W Speaker (34.8mm *11.2mm *6.0mm) | 1 | $3.50 | [AliExpress Search](https://www.aliexpress.com/item/33035261832.html) |
| IP5306 5V 2A USB-C Charging & Boost Board (26 × 19 mm) | 1 | $2.50 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-IP5306-5V-2A-USB-C-charging-boost-module.html?spm=a2g0o.productlist.search.0) |
| 3.7V Single-Cell LiPo Pouch 3,500 mAh | 1 | $9.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-3.7V-single-cell-LiPo-battery-3500mah.html?spm=a2g0o.productlist.search.0) |
| Neodymium Disc Magnets (5 × 2 mm) | 4 | $2.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-neodymium-magnets-5x2mm.html?spm=a2g0o.productlist.search.0) |
| M2.5 Machine Screws | 8 | $1.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-M2-machine-screws-5mm.html?spm=a2g0o.productlist.search.0) |
| M2.7.5 Machine Screws | 8 | $1.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-M2.75-machine-screws-5mm.html?spm=a2g0o.productlist.search.0) |

## Image

<img width="376" height="485" alt="image" src="https://github.com/user-attachments/assets/3412c88f-ecd3-487a-a12a-d51cd59bb02b" />

<img width="393" height="490" alt="image" src="https://github.com/user-attachments/assets/ba8c89dd-6cac-4772-a176-e4bb7754fda9" />

<img width="426" height="464" alt="image" src="https://github.com/user-attachments/assets/2ffd8831-f01a-49d9-803b-76c02174a191" />

<img width="516" height="507" alt="image" src="https://github.com/user-attachments/assets/0067a1cf-4562-4f55-bd36-ec8e64367d49" />

## Notes

This project is for Fabricate a YSWS program by HackClub
https://fabricate.hackclub.com/projects/1226
