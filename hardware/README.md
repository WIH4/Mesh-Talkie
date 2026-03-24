# Hardware

> **Status: Not started.** Schematics and PCB files will be added when Stage 2 (PCB v0.1) begins.

Planned PCB design tool: EasyEDA PRO. All components SMD, factory-assembled via JLCPCB SMT service — flash firmware, connect antenna and battery, insert into enclosure.

## PCB Specification

| Parameter | Value |
|---|---|
| Size | ~80 × 45 mm |
| Layers | 2 (TBD — 4 layers for RF section) |
| Form factor | Baofeng UV-5R compatible (119 × 57 × 35 mm enclosure) |
| RF connector | SMA female |
| Power connector | USB-C — charging (MCP73831T) + programming (CH340C), single cable |
| Audio jack | 3.5 mm TRRS |

## Key Components

| Ref | Part | Package | Source |
|---|---|---|---|
| U1 | ESP32-S3-MINI-1 | LCC | Espressif |
| U2 | SX1262 | QFN-24 | Semtech |
| U3 | MAX98357A | WLP | Maxim |
| U4 | TPS63020 | VSON-10 | TI |
| U5 | MCP73831T-2ACI/OT | SOT-23-5 | LCSC |
| U8 | CH340C | SOP-16 | LCSC |
| MIC1 | SPH0645LM4H | LGA-5 | Knowles |
| LCD1 | OLED 0.96" SSD1306 | Module | — |

Full BOM: [`../docs/bom.md`](../docs/bom.md)

## Ordering (JLCPCB)

When Gerbers are released:
1. Upload `gerber.zip` to jlcpcb.com
2. Enable **SMT Assembly**, upload BOM and CPL files
3. Select components from JLCPCB parts library (most are in stock)
4. Manual soldering required for: SMA connector, speaker, battery connector, buttons

## Enclosure

- PoC: 3D printed PLA/PETG (STL files will be in this folder)
- Target: modified Baofeng UV-5R enclosure or custom CNC aluminium

## Status

Stage 2 (PCB v0.1) — not yet started. Schematic and layout will appear here after PoC validation.
