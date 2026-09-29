---
title: Transmit Power Per Device
date: 2026-09-29
author: Martin
draft: true
summary: Application setting of transmit power to actual output power for
  meshcore compannions
---
# MeshCore TX power: does the setting match the output?

MeshCore sends the app's TX power number straight to the radio chip. It has no
per-board correction. The companion firmware accepts **-9 up to the board's
max**, and the device reports that max to the app.

## ✅ Setting = output (no external amplifier)

A setting of 22 really is ~22 dBm. Every value from -9 to 22 works.

- **Heltec:** V3, Tracker (v1), Wireless Paper, CT62, E213, E290, Mesh Solar,
  RC32, T1, T114, T190
- **LilyGo:** T-Beam SX1262, T-Beam Supreme, T-Deck, T-Echo (all versions),
  T3S3 SX1262, T-Impulse Plus, T-LoRa C6
- **RAK:** RAK4631 (without booster), RAK11310, RAK3112, WisMesh Tag
- **Seeed:** Xiao kits (C3, C6, nRF52, RP2040, S3, S3 Wio), Wio Tracker L1,
  SenseCAP Solar
- **ThinkNode:** M1, M2, M5, M6
- **Others:** Ebyte EoRa-S3, GAT562 EVB Pro / Tracker Pro / Watch13 (max 19),
  Keepteen LT1, M5Stack C6L, Mesh Pocket, Meshtiny, Muziworks R1 Neo,
  Nano G2 Ultra, Nibble, ProMicro DIY (without high-power E22), RPi Pico W,
  Tenstar C3, Waveshare RP2040, Ikoka nano/stick **22 dBm builds**

These also match, with small quirks:
- **LR1110 chip** (T1000-E, ThinkNode M3/M7/M9, Wio WM1110, Minew ME25LS01):
  14 and below uses the efficient low-power amplifier; 15 and up switches to
  the high-power amplifier and roughly doubles the current.
- **STM32WL** (RAK3x72, Tiny Relay, Wio-E5): matches.
- **LR2021** (MeshTracker X1): matches.

## ⚠️ Setting = output, but some values don't work (SX1276)

**Heltec V2, T-Beam SX1276, T3S3 SX1276, T-LoRa V2.1** (max 20)

Only **2–17 and 20** work. Negatives, 0, 1, 18 and 19 are ignored by the
radio, but the app still shows the new number.

## ❌ Setting ≠ output (external amplifier)

| Board | Max setting | ≈ Output at max |
|---|---|---|
| Heltec V4 / V4 R8 | 22 | 28–29 dBm (measured) |
| Heltec Tracker V2, T096, Tower V2 | 22 | ~28–29 dBm |
| LilyGo T-Beam 1W | 22 | ~32 dBm |
| RAK3401 + RAK13302 booster | 22 | ~30 dBm |
| GAT562 30S | 22 | ~30 dBm |
| Generic E22 / MeshAdventurer (E22-M30S) | 22 | ~30 dBm |
| Ikoka **30 dBm** builds | 20 | ~27–28 dBm |
| Ikoka **33 dBm** builds | 9 | ~33 dBm |
| Station G2 | 19 | >30 dBm (7 → ~27 dBm) |
| Station G3 | 22 | depends on PA jumper |
| Meshnology W12 | 4 | ~28–30 dBm |

Rule of thumb: output ≈ setting + 7 to 14 dB. The gain shrinks near the top as
the amplifier maxes out.

## ❓ Unknown

- **LilyGo T-ETH Elite:** capped at 8 with no explanation. Possibly a
  high-power shield.

## For the app

- ✅ boards: nothing to change.
- ⚠️ SX1276 boards: limit the field to 2–17 and 20.
- ❌ boards: "dBm" is misleading. Show "≈ X dBm at antenna" (keyed on the
  device's model string), or call it "drive level".
# MeshCore TX power: does the setting match the output?

MeshCore sends the app's TX power number straight to the radio chip. It has no
per-board correction. The companion firmware accepts **-9 up to the board's
max**, and the device reports that max to the app.

## ✅ Setting = output (no external amplifier)

A setting of 22 really is ~22 dBm. Every value from -9 to 22 works.

- **Heltec:** V3, Tracker (v1), Wireless Paper, CT62, E213, E290, Mesh Solar,
  RC32, T1, T114, T190
- **LilyGo:** T-Beam SX1262, T-Beam Supreme, T-Deck, T-Echo (all versions),
  T3S3 SX1262, T-Impulse Plus, T-LoRa C6
- **RAK:** RAK4631 (without booster), RAK11310, RAK3112, WisMesh Tag
- **Seeed:** Xiao kits (C3, C6, nRF52, RP2040, S3, S3 Wio), Wio Tracker L1,
  SenseCAP Solar
- **ThinkNode:** M1, M2, M5, M6
- **Others:** Ebyte EoRa-S3, GAT562 EVB Pro / Tracker Pro / Watch13 (max 19),
  Keepteen LT1, M5Stack C6L, Mesh Pocket, Meshtiny, Muziworks R1 Neo,
  Nano G2 Ultra, Nibble, ProMicro DIY (without high-power E22), RPi Pico W,
  Tenstar C3, Waveshare RP2040, Ikoka nano/stick **22 dBm builds**

These also match, with small quirks:
- **LR1110 chip** (T1000-E, ThinkNode M3/M7/M9, Wio WM1110, Minew ME25LS01):
  14 and below uses the efficient low-power amplifier; 15 and up switches to
  the high-power amplifier and roughly doubles the current.
- **STM32WL** (RAK3x72, Tiny Relay, Wio-E5): matches.
- **LR2021** (MeshTracker X1): matches.

## ⚠️ Setting = output, but some values don't work (SX1276)

**Heltec V2, T-Beam SX1276, T3S3 SX1276, T-LoRa V2.1** (max 20)

Only **2–17 and 20** work. Negatives, 0, 1, 18 and 19 are ignored by the
radio, but the app still shows the new number.

## ❌ Setting ≠ output (external amplifier)

| Board | Max setting | ≈ Output at max |
|---|---|---|
| Heltec V4 / V4 R8 | 22 | 28–29 dBm (measured) |
| Heltec Tracker V2, T096, Tower V2 | 22 | ~28–29 dBm |
| LilyGo T-Beam 1W | 22 | ~32 dBm |
| RAK3401 + RAK13302 booster | 22 | ~30 dBm |
| GAT562 30S | 22 | ~30 dBm |
| Generic E22 / MeshAdventurer (E22-M30S) | 22 | ~30 dBm |
| Ikoka **30 dBm** builds | 20 | ~27–28 dBm |
| Ikoka **33 dBm** builds | 9 | ~33 dBm |
| Station G2 | 19 | >30 dBm (7 → ~27 dBm) |
| Station G3 | 22 | depends on PA jumper |
| Meshnology W12 | 4 | ~28–30 dBm |

Rule of thumb: output ≈ setting + 7 to 14 dB. The gain shrinks near the top as
the amplifier maxes out.

## ❓ Unknown

- **LilyGo T-ETH Elite:** capped at 8 with no explanation. Possibly a
  high-power shield.

## For the app

- ✅ boards: nothing to change.
- ⚠️ SX1276 boards: limit the field to 2–17 and 20.
- ❌ boards: "dBm" is misleading. Show "≈ X dBm at antenna" (keyed on the
  device's model string), or call it "drive level".
