# Standalone Mesh Protocol

Protocol used in standalone mode (no Reticulum stack). Inspired by Reticulum where compatible.

## Addressing

| Field | Size | Description |
|---|---|---|
| Device ID | 32 bit | Derived from ESP32 eFuse / MAC hash |
| Group ID | 16 bit | Channel / group |
| Broadcast | 0xFFFF | All devices in range |

## Frame Format

Maximum payload: 255 bytes (GFSK), ~220 bytes effective (LoRa SF7 BW125kHz).

```
[Preamble 4B][Sync 2B][From 4B][To 4B][Group 2B][TTL 1B]
[Type 1B][Seq 2B][PayloadLen 1B][Payload N B][CRC16 2B]
```

### Frame Types

| Type | Value | Description |
|---|---|---|
| `VOICE_FRAME` | 0x01 | Codec2 audio payload (5 frames × 16 B = 80 B) |
| `VOICE_END` | 0x02 | PTT release signal |
| `TEXT` | 0x03 | UTF-8 text message |
| `GPS` | 0x04 | Location beacon |
| `BEACON` | 0x05 | Device presence / keepalive |
| `KEY_EXCH` | 0x06 | Group key exchange (via BLE/WiFi, not radio) |

## Routing

- Flood with TTL decrement (max TTL = 5)
- Deduplication: cache of last 64 packets by (From + Seq)
- v2.0 plan: AODV routing for networks > 10 nodes

## Encryption

| Channel | Method |
|---|---|
| Private group | AES-256-CTR, 256-bit PSK, group key |
| Authentication | HMAC-SHA256 on every frame |
| Key exchange | QR code over BLE (not transmitted over radio) |
| Open channel | No encryption, explicitly marked in frame header |

## Duty Cycle Manager (869.4 MHz)

- Sliding 3600 s window
- Maximum TX: 360 s (10%)
- Warning threshold: < 30 s reserve → yellow LED + buzzer
- Hard block: TX disabled, red LED
- Window resets incrementally each minute
