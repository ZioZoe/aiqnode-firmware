# AIQnode firmware

Free firmware that turns an ESP32, ESP32-S3 or ESP8266 board into an [AIQnode](https://aiqnode.com) device. Flash it once, then set up Wi-Fi, pins, dashboards and automations from the AIQnode app.

This repository holds the **compiled firmware files only** (in the [`bin/`](bin) folder and in [Releases](../../releases)). The source code is not published.

## Install from your browser (easiest)

Open **https://aiqnode.com/flash/**, plug your board into your computer with a USB data cable, and press **Install AIQnode firmware**. It works in Chrome or Edge on a computer and detects your board by itself. No software to install.

## Download

Latest version: **0.3.1** — [open the latest release](../../releases/latest)

| Board | File | SHA-256 |
|---|---|---|
| ESP32 (DevKit, WROOM) | `aiqnode-esp32-0.3.1.bin` | `70f1ad5cffc570486162b662eb1f8223a67b0c6e301a32b78511ca2c1cf8fb73` |
| ESP32-S3 (DevKitC) | `aiqnode-esp32s3-0.3.1.bin` | `709a7d29fe66cf78a17ce796d8e3b7c04ab74ac0080aa00ab5e3f811e639091e` |
| ESP8266 (NodeMCU, Wemos D1 mini) | `aiqnode-esp8266-0.3.1.bin` | `7e1d8fc4d08460b40da72fc4a7ffb84b3f1d5c00a7efd004bf167448c31221b1` |

Not sure which board you have? Look at the text printed on the chip shield: **ESP32-S3** means the S3 file, **ESP8266** means the ESP8266 file, any other ESP32 means the ESP32 file. ESP32-C3, C6 and S2 are not supported yet.

## Flash a downloaded file

The full step-by-step guide with troubleshooting is at **https://aiqnode.com/flash/**.

With [esptool](https://docs.espressif.com/projects/esptool/):

```
esptool.py --chip esp32   --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32-0.3.1.bin
esptool.py --chip esp32s3 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32s3-0.3.1.bin
esptool.py --chip esp8266 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp8266-0.3.1.bin
```

Or in Chrome or Edge open <https://espressif.github.io/esptool-js/>, connect your board, set the flash address to `0x0`, choose the `.bin` for your board and press Program. Check a download with `sha256sum <file>` and compare it with the table above.

## What is new in 0.3.1

- Every output switched on is switched off again after at most one hour, even if the phone goes away.
- A board that is already set up only accepts new Wi-Fi settings after you tap its BOOT button once, so nobody in range of the setup hotspot can take it over.
- Pin 0 (the BOOT button) can no longer be used as an output.

## After flashing

The board starts a Wi-Fi hotspot named `AIQnode-XXXX`. Join it with your phone, choose your home Wi-Fi on the setup page and note the pairing code. Then add the board in the AIQnode app.

## Terms

Free to use on boards you own. You may share the unmodified files with the checksums above. No warranty; use at your own risk. See [LICENSE](LICENSE). AIQnode and its logo are trademarks of ZioZoe Software Labs.

Questions: support@aiqnode.com
