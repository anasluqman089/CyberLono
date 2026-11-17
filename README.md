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
| :--- | :--- | :--- |
| **Power** | 3.7V LiPo Battery (`+`) | IP5306 Module (`B+`) |
| **Power** | 3.7V LiPo Battery (`-`) | IP5306 Module (`B-`) |
| **Power** | IP5306 Module (`5V`) | Pi Zero 2 W (`Pin 2` or `Pin 4` - 5V) |
| **Power** | IP5306 Module (`GND` or `OUT-`) | Pi Zero 2 W (`Pin 6` - GND) |
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

Note : CardKB uses a 4-pin HY2.0-4P connector. So use a 4-pin HY2.0-4P to dupont wire adapter

## BOM 

| Componets | Qty | Price (USD) | Buy Link |
| :--- | :--- | :--- | :--- |
| Raspberry Pi Zero 2 W | 1 | $35.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-Raspberry-Pi-Zero-2-W.html?spm=a2g0o.productlist.search.0) |
| 3.5" SPI Touchscreen Display | 1 | $15.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-3.5-inch-Raspberry-Pi-display.html?spm=a2g0o.productlist.search.0) |
| M5Stack CardKB (I2C Mini QWERTY Keyboard) | 1 | $12.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-M5Stack-CardKB.html?spm=a2g0o.productlist.search.0) |
| MAX98357A I2S 3W Class D Amp Breakout | 1 | $4.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-MAX98357A-I2S-amplifier.html?spm=a2g0o.productlist.search.0) |
| Ghxamp Mini Speaker Cavity Loudspeaker 8Ohm 1W Speaker (34.8mm *11.2mm *6.0mm) | 1 | $3.50 | [AliExpress Search](https://www.aliexpress.com/item/33035261832.html) |
| IP5306 5V 2A USB-C Charging & Boost Board (26 × 19 mm) | 1 | $2.50 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-IP5306-5V-2A-USB-C-charging-boost-module.html?spm=a2g0o.productlist.search.0) |
| 3.7V Single-Cell LiPo Pouch 3,500 mAh | 1 | $9.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-3.7V-single-cell-LiPo-battery-3500mah.html?spm=a2g0o.productlist.search.0) |
| Neodymium Disc Magnets (5 × 2 mm) | 4 | $2.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-neodymium-magnets-5x2mm.html?spm=a2g0o.productlist.search.0) |
| M2.5 Machine Screws | 8 | $1.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-M2-machine-screws-5mm.html?spm=a2g0o.productlist.search.0) |
| M2.7.5 Machine Screws | 8 | $1.00 | [AliExpress Search](https://www.aliexpress.com/w/wholesale-M2.75-machine-screws-5mm.html?spm=a2g0o.productlist.search.0) |
| **Total** | — | **$85.00** | — |

## Initilizing the cyberdeck

### 1. Wiring & Pinout Table

Follow th wring guide given.

### 2. Software Initialization (Single Terminal Script)

Run this single command block in your Raspberry Pi OS terminal to enable I2C/I2S, install dependencies, create the CardKB driver, and configure auto-start on boot:

    
    sudo bash -c '
    cat << "EOF" >> /boot/firmware/config.txt
      
    # Cyberdeck Hardware Overlays
    dtparam=i2c_arm=on
    dtparam=i2s=on
    dtoverlay=hifiberry-dac
    dtoverlay=i2s-mmap
    EOF
        
    apt-get update && apt-get install -y python3-smbus i2c-tools python3-evdev
    modprobe uinput
    grep -qxF "uinput" /etc/modules || echo "uinput" >> /etc/modules
        
    cat << "EOF" > /home/pi/cardkb_driver.py
    import time
    import smbus2
    from evdev import UInput, ecodes as e
        
    I2C_BUS = 1
    CARDKB_ADDR = 0x5F
        
    KEY_MAP = {
        0x08: e.KEY_BACKSPACE, 0x09: e.KEY_TAB, 0x0D: e.KEY_ENTER,
        0x1B: e.KEY_ESC, 0x20: e.KEY_SPACE, 0xB4: e.KEY_LEFT,
        0xB5: e.KEY_UP, 0xB6: e.KEY_DOWN, 0xB7: e.KEY_RIGHT,
    }
    for ch in "abcdefghijklmnopqrstuvwxyz":
        KEY_MAP[ord(ch)] = getattr(e, f"KEY_{ch.upper()}")
    for ch in "0123456789":
        KEY_MAP[ord(ch)] = getattr(e, f"KEY_{ch}")
        
    SHIFT_MAP = {
        "!": e.KEY_1, "@": e.KEY_2, "#": e.KEY_3, "$": e.KEY_4, "%": e.KEY_5,
        "^": e.KEY_6, "&": e.KEY_7, "*": e.KEY_8, "(": e.KEY_9, ")": e.KEY_0,
        "_": e.KEY_MINUS, "+": e.KEY_EQUAL, "{": e.KEY_LEFTBRACE, "}": e.KEY_RIGHTBRACE,
        "|": e.KEY_BACKSLASH, ":": e.KEY_SEMICOLON, "\"": e.KEY_APOSTROPHE,
        "<": e.KEY_COMMA, ">": e.KEY_DOT, "?": e.KEY_SLASH, "~": e.KEY_GRAVE,
    }
    for ch in "ABCDEFGHIJKLMNOPQRSTUVWXYZ":
        SHIFT_MAP[ch] = getattr(e, f"KEY_{ch}")
   
    PUNCT_MAP = {
        "-": e.KEY_MINUS, "=": e.KEY_EQUAL, "[": e.KEY_LEFTBRACE, "]": e.KEY_RIGHTBRACE,
        "\\": e.KEY_BACKSLASH, ";": e.KEY_SEMICOLON, "\x27": e.KEY_APOSTROPHE,
        ",": e.KEY_COMMA, ".": e.KEY_DOT, "/": e.KEY_SLASH, "`": e.KEY_GRAVE,
    }
 
    bus = smbus2.SMBus(I2C_BUS)
    ui = UInput()

    def press_key(keycode, shift=False):
        if shift:
            ui.write(e.EV_KEY, e.KEY_LEFTSHIFT, 1)
        ui.write(e.EV_KEY, keycode, 1)
        ui.write(e.EV_KEY, keycode, 0)
        if shift:
            ui.write(e.EV_KEY, e.KEY_LEFTSHIFT, 0)
        ui.syn()

    while True:
        try:
            data = bus.read_byte(CARDKB_ADDR)
            if data != 0x00:
                char = chr(data)
                if data in KEY_MAP:
                    press_key(KEY_MAP[data])
                elif char in SHIFT_MAP:
                    press_key(SHIFT_MAP[char], shift=True)
                elif char in PUNCT_MAP:
                    press_key(PUNCT_MAP[char])
        except Exception:
            pass
        time.sleep(0.02)
    EOF

    chown pi:pi /home/pi/cardkb_driver.py
    (crontab -l 2>/dev/null; echo "@reboot sleep 5 && python3 /home/pi/cardkb_driver.py &") | crontab -
    reboot
    '

## 4. Verification Commands

* **Check CardKB I2C (`5f` should appear):** `i2cdetect -y 1`
* **Test MAX98357A Speaker Audio:** `speaker-test -c2 -twav`

## Image

<img width="376" height="485" alt="image" src="https://github.com/user-attachments/assets/3412c88f-ecd3-487a-a12a-d51cd59bb02b" />

<img width="393" height="490" alt="image" src="https://github.com/user-attachments/assets/ba8c89dd-6cac-4772-a176-e4bb7754fda9" />

<img width="426" height="464" alt="image" src="https://github.com/user-attachments/assets/2ffd8831-f01a-49d9-803b-76c02174a191" />

<img width="516" height="507" alt="image" src="https://github.com/user-attachments/assets/0067a1cf-4562-4f55-bd36-ec8e64367d49" />

## Notes

This project is for Fabricate a YSWS program by HackClub
https://fabricate.hackclub.com/projects/1226
