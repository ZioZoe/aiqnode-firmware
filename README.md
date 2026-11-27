# AIQnode firmware

Free firmware that turns an ESP32, ESP32-S3 or ESP8266 board into an [AIQnode](https://aiqnode.com) device. Flash it once, then set up Wi-Fi, pins, dashboards and automations from the AIQnode app.

This repository holds the **compiled firmware files only** (in the [`bin/`](bin) folder and in [Releases](../../releases)). The source code is not published.

## Install from your browser (easiest)

Open **https://aiqnode.com/flash/**, plug your board into your computer with a USB data cable, and press **Install AIQnode firmware**. It works in Chrome or Edge on a computer and detects your board by itself. No software to install.

## Download

Latest version: **0.3.9** — [open the latest release](../../releases/latest)

| Board | File | SHA-256 |
|---|---|---|
| ESP32 (DevKit, WROOM) | `aiqnode-esp32-0.3.9.bin` | `1fc134d0a8551df34dd549b20cf354bcb3cec970780b5d4e2327c81b2950c516` |
| ESP32-S3 (DevKitC) | `aiqnode-esp32s3-0.3.9.bin` | `237a30996e3d3247ea24fdcd7064b1f438c1eafced3de0401b3aa06fa8696467` |
| ESP8266 (NodeMCU, Wemos D1 mini) | `aiqnode-esp8266-0.3.9.bin` | `87fa8ca720b1b8d005772de871ea2c537c53f827cbbb99caaac1c32c18334f6f` |

Not sure which board you have? Look at the text printed on the chip shield: **ESP32-S3** means the S3 file, **ESP8266** means the ESP8266 file, any other ESP32 means the ESP32 file. ESP32-C3, C6 and S2 are not supported yet.

## Flash a downloaded file

The full step-by-step guide with troubleshooting is at **https://aiqnode.com/flash/**.

With [esptool](https://docs.espressif.com/projects/esptool/):

```
esptool.py --chip esp32   --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32-0.3.9.bin
esptool.py --chip esp32s3 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32s3-0.3.9.bin
esptool.py --chip esp8266 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp8266-0.3.9.bin
```

Or in Chrome or Edge open <https://espressif.github.io/esptool-js/>, connect your board, set the flash address to `0x0`, choose the `.bin` for your board and press Program. Check a download with `sha256sum <file>` and compare it with the table above.

## What is new in 0.3.9

- The board keeps its Wi-Fi radio awake, so phones reach it reliably. Some routers stopped passing the phone's requests to a sleeping board: the app found the board but could not connect.
- The Wi-Fi setup page says the board uses 2.4 GHz only and warns when a guest network is chosen.
- Updates keep your settings on ESP32 and ESP32-S3: the browser installer now writes two pieces that skip the settings area. For esptool, use the `-boot.bin` and `-app.bin` files of this release: `write_flash 0x0 aiqnode-esp32-0.3.9-boot.bin 0xe000 aiqnode-esp32-0.3.9-app.bin`. The single `.bin` is for first installs; written at 0x0 it also clears the Wi-Fi, pairing code and components.

## What is new in 0.3.8

- Fixed a white Wi-Fi setup page: the board now sends the page straight from flash instead of copying it into memory, which often failed on ESP8266 boards with the hotspot running.

## What is new in 0.3.7

- Components keep a stable id, so rules, corrections and dashboard tiles stay with the right component when another one is removed, switched off or changes type. Boards set up earlier keep their ids.
- A switched-off component stays on the board with its pin and settings instead of being deleted.
- Timed runs switch off on the board at the right time, even when the phone is gone.
- Analog inputs never read above 100 %.
- PWM outputs and servos on ESP32 boards each get their own channel (at most 4 of each); ESP8266 GPIO 15 refuses an inverted output that would stop the board from starting.
- The board reports its firmware version with its component list.

Works best with the AIQnode app update that came with it.

## What is new in 0.3.6

- Remote control works again: boards connected to the cloud now act on commands sent from outside the home network.
- The Wi-Fi setup page shows the new AIQnode logo.
- The board refuses a board type made for a different chip, so pin rules for the wrong chip can no longer reach dangerous pins.
- ESP8266 GPIO 16 (D0) is no longer accepted for DHT or DS18B20 sensors (it cannot release their data line).
- Wider value ranges: DHT22 -40 to 80 °C, DS18B20 -55 to 125 °C, HC-SR04 up to 450 cm.

## What is new in 0.3.5

- **DS18B20** waterproof temperature probe (one sensor per pin, 4.7 kOhm pull-up).
- **HC-SR04** ultrasonic distance sensor (Trig and Echo pins; the 5 V Echo needs a 1k/2k divider).
- **Servo motors** (SG90, MG996R), set to an angle from 0 to 180°.
- Analog inputs can be shown in real units (value at 0 V and at 3.3 V, and a unit).

Needs the AIQnode app update that adds these components.

## What is new in 0.3.4

- DHT11 and DHT22 sensors now give readings: the reader was rewritten (it used to drop every reading).
- If a DHT sensor gives no reading, the board says so in its log (Logs & Console on aiqnode.com/flash) about every 30 seconds.
- Buttons and switches are refused on pins where they cannot work (no pull-up, or a pull-down on the board), and DHT sensors on ESP8266 GPIO 15 (D8), which would stop the board from starting.
- Boards with many components publish their full component list to the cloud.

## What is new in 0.3.3

- New outputs: **Digital output** (on/off), **PWM output** (0 to 100 %, for dimming LEDs or setting fan and motor speed through a driver) and **Analog output** (a real 0 to 3.3 V voltage, classic ESP32 on GPIO 25 and 26 only).
- Pins the board needs for itself are locked: GPIO 0 (BOOT/FLASH button) on all boards, GPIO 1, 3 (USB serial) and 12 (flash voltage) on the ESP32, and GPIO 33 to 37 (PSRAM) on the ESP32-S3.
- The one-hour automatic switch-off also covers PWM and analog outputs.

Needs the AIQnode app update that adds these outputs.

## What is new in 0.3.2

- New components: **Digital input** (button, door or reed switch, PIR motion sensor; built-in pull-up and an Invert option), **Analog input** (0 to 100%) and the **DHT11** temperature and humidity sensor.
- Two DHT sensors on one board now each show their own readings.
- The board tells the app its board type and firmware version on the network, so the app's device search shows the right board picture and details.
- Pin 0 (the BOOT button) and the ESP8266 A0 pin cannot be used as digital inputs.

Needs the AIQnode app update that adds these components. Older app versions keep working with this firmware.

## What is new in 0.3.1

- Every output switched on is switched off again after at most one hour, even if the phone goes away.
- A board that is already set up only accepts new Wi-Fi settings after you tap its BOOT button once, so nobody in range of the setup hotspot can take it over.
- Pin 0 (the BOOT button) can no longer be used as an output.

## After flashing

The board starts a Wi-Fi hotspot named `AIQnode-XXXX`. Join it with your phone, choose your home Wi-Fi on the setup page and note the pairing code. Then add the board in the AIQnode app.

## Terms

Free to use on boards you own. You may share the unmodified files with the checksums above. No warranty; use at your own risk. See [LICENSE](LICENSE). AIQnode and its logo are trademarks of ZioZoe Software Labs.

Questions: support@aiqnode.com
