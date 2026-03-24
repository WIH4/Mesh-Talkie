# Regulatory Summary — EU

## Applicable regulations

| Regulation | Scope |
|---|---|
| ERC/REC 70-03 (2024) | EU SRD frequency allocations and limits |
| ETSI EN 300 220-2 | Technical parameters for SRD devices |
| RED 2014/53/EU | Radio Equipment Directive (CE marking) |

## Frequency allocation used

| Sub-band | Power | Duty cycle | Basis |
|---|---|---|---|
| 868.0 MHz | 25 mW ERP | 1% | ERC/REC 70-03 Band h1 |
| 869.4–869.65 MHz | 500 mW ERP | 10% | ERC/REC 70-03 Band h2.6 |

Both sub-bands fall within the same physical frequency range (~1.4 MHz apart). A single 868 MHz antenna covers both (VSWR < 1.2:1 at 869.4 MHz).

## License requirement

**None** — both sub-bands are classified as Short Range Device (SRD) allocations. Operation within the specified power and duty cycle limits does not require a radio license anywhere in the EU where ERC/REC 70-03 is adopted (all EU member states).

## CE marking (RED 2014/53/EU)

| Scenario | Requirement |
|---|---|
| DIY build / personal use | Not required |
| Open source kit (user assembles) | Explicitly excluded from RED scope |
| Selling assembled, ready-to-use devices | CE marking required |

Self-certification (Module A) is possible without a Notified Body. Lab testing for CE: ~2,000–8,000 EUR (relevant only at commercialization stage).

RED applies to radio equipment **placed on the market** (commercial sale). Building for personal use or distributing as an unassembled kit falls outside that scope. Selling assembled, ready-to-use devices requires CE marking regardless of open source status.

## Cybersecurity (RED Art. 3.3, from August 2025)

Applies to devices with internet connectivity. Mesh-Talkie is peer-to-peer radio — likely outside scope. Firmware updates via BLE/WiFi should be reviewed by legal counsel before commercial sale.

## References

- ERC/REC 70-03 (2024): https://docdb.cept.org/download/4635
- ETSI EN 300 220-2: https://www.etsi.org/deliver/etsi_en/300200_300299/30022002/03.03.01_60/en_30022002v030301p.pdf
- RED 2014/53/EU: https://single-market-economy.ec.europa.eu/sectors/electrical-and-electronic-engineering-industries-eei/radio-equipment-directive-red_en
