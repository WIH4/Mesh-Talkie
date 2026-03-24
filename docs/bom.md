# Bill of Materials

Preliminary BOM for custom PCB. Prices approximate, sourced from LCSC / Mouser (2024).

## Core Components

| Qty | Reference | Part | Package | ~Unit Price | Source |
|---|---|---|---|---|---|
| 1 | U1 | ESP32-S3-MINI-1U (8 MB flash) | LCC | $3.20 | LCSC / Espressif |
| 1 | U2 | SX1262 | QFN-24 | $4.50 | LCSC / Semtech |
| 1 | U3 | MAX98357AEWL+T | WLP | $2.10 | LCSC / Maxim |
| 1 | U4 | TPS63020DSJT | VSON-10 | $2.80 | LCSC / TI |
| 1 | U5 | MCP73831T-2ACI/OT | SOT-23-5 | $0.40 | LCSC / Microchip |
| 1 | U8 | CH340C | SOP-16 | $0.30 | LCSC |
| 1 | U6 | DW01A | SOT-23-6 | $0.08 | LCSC |
| 1 | Q1 | FS8205A | SOT-23-6 | $0.08 | LCSC |
| 1 | MIC1 | SPH0645LM4H-B | LGA-5 | $1.80 | LCSC / Knowles |
| 1 | LCD1 | OLED 0.96" 128×64 SSD1306 (I2C) | Module | $2.00 | AliExpress |
| 1 | ANT1 | SMA female edge connector | — | $0.50 | LCSC |
| 1 | J1 | USB-C receptacle (16-pin) | SMD | $0.30 | LCSC |
| 1 | J2 | 3.5 mm TRRS jack | SMD | $0.40 | LCSC |
| 1 | SP1 | Speaker 28 mm, 8 Ω, 1 W | — | $1.20 | AliExpress |
| 1 | BT1 | Li-Po 3.7 V 3000 mAh flat | — | $4.00 | AliExpress |
| 1 | SW1 | PTT tact switch (side-mount) | — | $0.20 | LCSC |

## Optional — LNA (RX sensitivity improvement)

| Qty | Reference | Part | Notes | ~Unit Price |
|---|---|---|---|---|
| 1 | U7 | SPF5189Z | +16 dB gain, 0.6 dB NF | $1.50 |
| 1 | SW2 | PE4259 or similar SPDT RF switch | PA bypass at TX | $0.80 |

LNA adds ~3–6 dB effective sensitivity gain → ~1.5–2× range improvement on RX.

> **USB-C (J1) handles two independent paths:**
> - VBUS → MCP73831T → Li-Po battery (charging)
> - D+/D− → CH340C → ESP32-S3 UART0 (programming, auto-reset via RTS/DTR)
> - CC1/CC2 → 5.1 kΩ → GND (USB-C device mode detection)

## Passives

~$2.50 total (resistors, capacitors, inductors — full list with PCB release).
Notable additions: 2× 5.1 kΩ (CC1/CC2 pull-down), 1× 2 kΩ R_prog (MCP73831 sets 500 mA charge current), 2× 100 nF (CH340C decoupling).

## Estimated Cost per Unit

| Configuration | BOM Cost |
|---|---|
| Core only (no LNA) | ~$23 |
| With LNA | ~$25 |
| + PCB (JLCPCB, 5 pcs) | +$3/unit |
| + SMT assembly (JLCPCB) | +$8–15/unit depending on component count |

These are component costs only, not including enclosure, antenna, or shipping.
