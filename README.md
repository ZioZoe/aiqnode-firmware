# AIQnode firmware

Free firmware that turns an ESP32, ESP32-S3 or ESP8266 board into an [AIQnode](https://aiqnode.com) device. Flash it once, then set up Wi-Fi, pins, dashboards and automations from the AIQnode app.

This repository holds the **compiled firmware files only** (in the [`bin/`](bin) folder and in [Releases](../../releases)). The source code is not published.

## Install from your browser (easiest)

Open **https://aiqnode.com/flash/**, plug your board into your computer with a USB data cable, and press **Install AIQnode firmware**. It works in Chrome or Edge on a computer and detects your board by itself. No software to install.

## Download

Latest version: **0.3.11** — [open the latest release](../../releases/latest)

| Board | File | SHA-256 |
|---|---|---|
| ESP32 (DevKit, WROOM) | `aiqnode-esp32-0.3.11.bin` | `3d021a4789463905659d371d5356e5055031a127a45ce75faf943e8732d093cc` |
| ESP32-S3 (DevKitC) | `aiqnode-esp32s3-0.3.11.bin` | `2515cd7b7975cf9ce21adfc2cedce63318e68e109c126a665947885e469c20f5` |
| ESP32-C3 (SuperMini, DevKitM-1) | `aiqnode-esp32c3-0.3.11.bin` | `69ef7a26a33521ba012db8b575e38b9a9d81a5bdb128ad4d7b10ba04985cf3cf` |
| ESP8266 (NodeMCU, Wemos D1 mini) | `aiqnode-esp8266-0.3.11.bin` | `e6643141e91ca0073b22c0583a4ffa0a26224c673964604c47fe8f58834c23bd` |

Not sure which board you have? Look at the text printed on the chip shield: **ESP32-S3** means the S3 file, **ESP32-C3** the C3 file, **ESP8266** means the ESP8266 file, any other ESP32 means the ESP32 file. ESP32-C6 and S2 are not supported yet.

## Flash a downloaded file

The full step-by-step guide with troubleshooting is at **https://aiqnode.com/flash/**.

With [esptool](https://docs.espressif.com/projects/esptool/):

```
esptool.py --chip esp32   --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32-0.3.11.bin
esptool.py --chip esp32s3 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32s3-0.3.11.bin
esptool.py --chip esp32c3 --port /dev/ttyACM0 write_flash 0x0 aiqnode-esp32c3-0.3.11.bin
esptool.py --chip esp8266 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp8266-0.3.11.bin
```

Or in Chrome or Edge open <https://espressif.github.io/esptool-js/>, connect your board, set the flash address to `0x0`, choose the `.bin` for your board and press Program. Check a download with `sha256sum <file>` and compare it with the table above.

## What is new in 0.3.11

- Rules run on the board: a rule that uses only one board's sensors and outputs is stored on the board and keeps working when the phone is off or away (needs the matching AIQnode app update). Switching an output by hand pauses its rules for 15 minutes.
- A sensor that stops answering (DHT, DS18B20, HC-SR04) is reported as no data instead of 0.

## What is new in 0.3.10

- ESP32-C3 support: ESP32-C3 SuperMini and ESP32-C3-DevKitM-1. The BOOT button is GPIO 9; on the SuperMini the log and pairing code come over its USB port. At most 3 PWM outputs and 3 servos.
- ESP32, ESP32-S3 and ESP32-C3 use a larger app area (1.9 MB, "Minimal SPIFFS" layout), so there is room for more sensors. Your settings are kept when you update from this page or with the `-boot.bin` and `-app.bin` pieces.

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
