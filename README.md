# Mesh-Talkie

Open-source LoRa mesh voice radio — operates without a radio license under EU SRD regulations (868/869 MHz).

> **Status: Planning / Pre-PoC** — No hardware built yet. No firmware written yet.
> This repository documents the design decisions and planned architecture.

---

## What it is

Mesh-Talkie is a planned open-source hand-held PTT radio built around ESP32-S3 and SX1262. It will transmit digitally encoded voice over a self-forming mesh network using LoRa modulation. Devices will relay packets automatically — range extends with each node added to the network.

The PCB will be fully SMD, factory-assembled via JLCPCB SMT service. End result: flash firmware, connect antenna and battery, insert into enclosure — done.

Two operating modes will share a single PCB:
- **Standalone** — works as a walkie-talkie with no phone required
- **RNode** — acts as a radio interface for [Reticulum](https://reticulum.network) via USB-C or Bluetooth, enabling full compatibility with [Sideband](https://github.com/markqvist/Sideband) and [MeshChat](https://github.com/liamcottle/reticulum-meshchat)

The project will be fully open source. Hardware under CERN-OHL-S v2, firmware under GPL-3.0.

---

## Planned Hardware Specification

| Component | Part | Notes |
|---|---|---|
| MCU | ESP32-S3-MINI-1 | Dual-core 240 MHz, I2S, SPI, BT 5.0 LE |
| RF | SX1262 | 868/869 MHz, LoRa + GFSK, −123 dBm sensitivity |
| Microphone | SPH0645LM4H | I2S MEMS (Knowles) |
| Speaker amp | MAX98357A | I2S Class-D, 3 W @ 4 Ω |
| Speaker | 28 mm, 8 Ω | Front-facing |
| Display | OLED 0.96" 128×64 | SSD1306, I2C |
| Battery | Li-Po 3.7 V 3000 mAh | 18650 or flat pack |
| Li-Po charger | MCP73831T | SOT-23-5, USB-C VBUS input |
| USB-UART | CH340C | SOP-16, programs via USB-C, auto-reset (RTS/DTR), no crystal |
| Regulator | TPS63020 | 3.3 V, 2 A |
| PCB tool | EasyEDA PRO | JLCPCB SMT assembly compatible |
| PCB size | ~80 × 45 mm | Baofeng UV-5R form factor |

**Voice codec:** Codec2 @ 3200 bps — real-time on ESP32-S3 at 240 MHz
**Encryption:** AES-256-CTR + HMAC-SHA256 per frame (group channels)
**LNA (optional):** SPF5189Z — +3–6 dB RX sensitivity gain

---

## Frequency Plan (EU, no license required)

| Sub-band | Use | Power | Duty cycle |
|---|---|---|---|
| 868.0 MHz | Control plane (routing, ACK, text) | 25 mW | 1% |
| 869.4–869.65 MHz | Voice (PTT frames) | 500 mW ERP | 10% |

Both sub-bands covered by a single 868 MHz antenna (VSWR < 1.2:1 at 869.4 MHz).
Modulation: **LoRa SF7 BW125kHz** — sensitivity −123 dBm, 5.5 kbps, ~25–40 km LoS at 500 mW.

Regulatory basis: ERC/REC 70-03 + ETSI EN 300 220 (SRD — Short Range Device). No radio license required across the EU. See [`docs/regulations-eu.md`](docs/regulations-eu.md).

---

## Planned Architecture

```
[MIC I2S] ──┐
            ├──► [ESP32-S3] ──── SPI ────► [SX1262] ──► [SMA 868 MHz]
[SPK I2S] ──┘        │
                      ├── I2C ──► [OLED 0.96"]
                      ├── UART ─► [GPS (optional)]
                      ├── GPIO ─► [PTT button]
                      ├── USB-C ► PC / Phone (RNode mode)
                      └── BT ───► Config app (Android BLE)
```

**PTT voice flow (standalone):**
1. PTT pressed → I2S capture, 40 ms buffer (320 samples @ 8 kHz)
2. Codec2 encode → 16 bytes / 40 ms
3. 5 frames packed → 1 radio packet (80 B payload)
4. Transmitted at 869.4 MHz, 500 mW, LoRa SF7
5. Intermediate nodes relay with TTL decrement
6. Receiver: Codec2 decode → MAX98357A → speaker

End-to-end latency per hop: ~200 ms (acceptable for PTT).

---

## Planned Operating Modes

### Standalone (walkie-talkie)
No phone, no internet. Mesh flood routing with TTL=5, deduplication cache (64 packets). Private group channels will use a 256-bit PSK exchanged via QR code over BLE. Open channel available without encryption.

Duty cycle manager will enforce the 10% limit on 869.4 MHz: sliding 3600 s window, LED + buzzer warning at <30 s reserve, TX blocked at limit.

### RNode (Reticulum interface)
Same PCB connected to a phone or PC via USB-C. Device will present itself as an RNode (RNode Serial Protocol v2). The host runs [Sideband](https://github.com/markqvist/Sideband) (Android/Linux) — full Reticulum stack with end-to-end encrypted voice, text, and GPS.

---

## Roadmap

- [x] Frequency plan decision (868/869 MHz, single antenna)
- [x] Regulatory research — EU SRD, ERC/REC 70-03, RED 2014/53/EU
- [x] Hardware architecture finalized
- [x] Protocol design (standalone mesh, frame format, duty cycle manager)
- [ ] **Stage 1 — PoC** (~4 weeks): TTGO LoRa32 V2 + SPH0645 breakout, Codec2 real-time, point-to-point voice over SX1262
- [ ] **Stage 2 — PCB v0.1** (~8 weeks): EasyEDA PRO schematic, JLCPCB 5 pcs, basic standalone firmware
- [ ] **Stage 3 — Alpha v1.0** (~12 weeks): RNode mode, OLED UI, GPS, AES-256 group encryption, Android BLE config app
- [ ] **Stage 4 — Open Source Release**: full schematics + Gerbers + BOM, ESP-IDF firmware, protocol documentation, JLCPCB SMT assembly option

---

## Getting Started

No code or hardware files exist yet — the project is in the planning stage.
This section will be updated when Stage 1 (PoC) begins.

Planned PoC hardware: TTGO LoRa32 V2 (~$15) + SPH0645LM4H breakout (~$3).

---

## License

- **Firmware:** [GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html)
- **Hardware (schematics, PCB, BOM):** [CERN-OHL-S v2](https://ohwr.org/cern_ohl_s_v2.txt)

---

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md).

---

I invite anyone interested to collaborate or to discuss this project or related topics.
