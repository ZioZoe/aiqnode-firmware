# AIQnode firmware

Free firmware that turns an ESP32, ESP32-S3 or ESP8266 board into an [AIQnode](https://aiqnode.com) device. Flash it once, then set up Wi-Fi, pins, dashboards and automations from the AIQnode app.

This repository holds the **compiled firmware files only** (in [Releases](../../releases)). The source code is not published.

## Download

Latest version: **0.3.0** — [open the latest release](../../releases/latest)

| Board | File | SHA-256 |
|---|---|---|
| ESP32 (DevKit, WROOM) | `aiqnode-esp32-0.3.0.bin` | `c42aa7fc619abc2484eb35118e5914ced7ce45288fb2abbefa8ba9687ba23aaa` |
| ESP32-S3 (DevKitC) | `aiqnode-esp32s3-0.3.0.bin` | `7af2b54411fc4eba8702c2e8c4b86940c26089b34949e7a20790957ec7e75860` |
| ESP8266 (NodeMCU, Wemos D1 mini) | `aiqnode-esp8266-0.3.0.bin` | `29d79b88672865feb1cfd1a0ae2604ab66ac16322f2631ad1d89591e711dfec3` |

Not sure which board you have? Look at the text printed on the chip shield: **ESP32-S3** means the S3 file, **ESP8266** means the ESP8266 file, any other ESP32 means the ESP32 file. ESP32-C3, C6 and S2 are not supported yet.

## Flash it

The full step-by-step guide with troubleshooting is at **https://aiqnode.com/flash/**.

In short, with a USB data cable and Chrome or Edge, open <https://espressif.github.io/esptool-js/>, connect your board, set the flash address to `0x0`, choose the `.bin` for your board and press Program.

Or with [esptool](https://docs.espressif.com/projects/esptool/):

```
esptool.py --chip esp32   --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32-0.3.0.bin
esptool.py --chip esp32s3 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp32s3-0.3.0.bin
esptool.py --chip esp8266 --port /dev/ttyUSB0 write_flash 0x0 aiqnode-esp8266-0.3.0.bin
```

Check a download with `sha256sum <file>` and compare it with the table above.

## After flashing

The board starts a Wi-Fi hotspot named `AIQnode-XXXX`. Join it with your phone, choose your home Wi-Fi on the setup page and note the pairing code. Then add the board in the AIQnode app.

## Terms

Free to use on boards you own. You may share the unmodified files with the checksums above. No warranty; use at your own risk. See [LICENSE](LICENSE). AIQnode and its logo are trademarks of ZioZoe Software Labs.

Questions: support@aiqnode.com
