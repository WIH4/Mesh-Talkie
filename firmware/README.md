# Firmware

> **Status: Not started.** Firmware will be added when Stage 1 (PoC) is complete.

Planned build environment: [ESP-IDF 5.x](https://docs.espressif.com/projects/esp-idf/en/latest/) in C/C++ using FreeRTOS.

## Dependencies

| Library | Source | Purpose |
|---|---|---|
| codec2 | [drowe67/codec2](https://github.com/drowe67/codec2) | Voice codec, 3200 bps |
| SX1262 driver | adapted from [RNode_Firmware](https://github.com/markqvist/RNode_Firmware) | LoRa/GFSK radio |
| mbedTLS | built into ESP-IDF | AES-256-CTR, HMAC-SHA256 |
| FreeRTOS | built into ESP-IDF | Audio pipeline, radio task |

## Operating modes

| Mode | Description |
|---|---|
| `STANDALONE` | Mesh walkie-talkie, no external device needed |
| `RNODE` | Radio interface for Reticulum via USB-C (RNode Serial Protocol v2) |

Mode is selected at boot via a GPIO pin or stored in NVS flash.

## Build (PoC — TTGO LoRa32 V2)

```bash
git clone https://github.com/WIH4/Mesh-Talkie.git
cd Mesh-Talkie/firmware

# Install ESP-IDF 5.x if not already installed
# https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/get-started/

idf.py set-target esp32s3
idf.py menuconfig   # set frequency, TX power, group key
idf.py build
idf.py -p COMx flash monitor
```

## Status

Stage 1 (PoC) — not yet started. Firmware will appear here once basic Codec2 + SX1262 point-to-point voice is working.
