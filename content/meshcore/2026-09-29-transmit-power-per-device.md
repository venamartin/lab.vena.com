---
title: Transmit Power Per Device
date: 2026-09-29
author: Martin
draft: true
summary: Application setting of transmit power to actual output power for
  meshcore compannions
---
# MeshCore TX power setting vs real output, by board

Working notes. Not part of the repo. Built 2026-09-29 from the MeshCore
firmware checkout (`meshtrax/MeshCore/variants/`, 87 variants) and RadioLib at
the commit MeshCore pins (`6d89348`). Amplifier gains are cross-checked
against Meshtastic's `src/configuration.h` and published measurements.

## How the setting works

- The number in the app (`CMD_SET_RADIO_TX_POWER`) goes straight to the radio
  chip: `setTxPower(dbm)` → RadioLib `setOutputPower(dbm)`. MeshCore has **no
  per-board gain table**.
- Companion firmware accepts **-9 … max**. "Max" is `MAX_LORA_TX_POWER`, or
  `LORA_TX_POWER` when a board doesn't define it. The device reports this max
  to the app.
- **No external amplifier:** setting = output at the chip. At the antenna it
  is typically 0.5–1 dB less (switch and matching loss).
- **External amplifier (PA/FEM):** setting = drive into the amplifier. The
  antenna gets roughly setting + gain, and the gain shrinks as the amplifier
  saturates.

---

## 1. Setting = output (no external amplifier), full -9 … 22 works

Radio: SX1262 / SX1268. RadioLib range -9 … 22. Every value the app allows
is applied as-is.

| Board (variant) | Default | App max | Notes |
|---|---|---|---|
| ebyte_eora_s3 | 22 | 22 | EoRa-S3 22 dBm module |
| gat562_mesh_evb_pro | 22 | 22 | |
| gat562_mesh_tracker_pro | 22 | 22 | |
| gat562_mesh_watch13 | 19 | **19** | max is 19 by config, no PA |
| heltec_ct62 | 22 | 22 | |
| heltec_e213 | 22 | 22 | |
| heltec_e290 | 22 | 22 | |
| heltec_mesh_solar | 22 | 22 | |
| heltec_rc32 | 22 | 22 | |
| heltec_t1 | 22 | 22 | |
| heltec_t114 | 22 | 22 | |
| heltec_t190 | 22 | 22 | |
| heltec_tracker (v1) | 22 | 22 | |
| heltec_v3 | 22 | 22 | |
| heltec_wireless_paper | 22 | 22 | |
| ikoka_nano_nrf / ikoka_stick_nrf — **22 dBm builds** | 22 | 22 | E22 without PA (the 30/33 dBm builds are in section 3) |
| keepteen_lt1 | 22 | 22 | |
| lilygo_t3s3 (SX1262) | 22 | 22 | |
| lilygo_t_impulse_plus | 22 | 22 | |
| lilygo_tbeam_SX1262 | 22 | 22 | |
| lilygo_tbeam_supreme_SX1262 | 22 | 22 | |
| lilygo_tdeck | 22 | 22 | |
| lilygo_techo / techo_card / techo_lite | 22 | 22 | |
| lilygo_tlora_c6 | 22 | 22 | |
| m5stack_unit_c6l | 22 | 22 | |
| mesh_pocket | 22 | 22 | |
| meshtiny | 22 | 22 | |
| muziworks_r1_neo | 22 | 22 | |
| nano_g2_ultra | 22 | 22 | |
| nibble_screen_connect / nibble_zero_connect | 22 | 22 | |
| promicro | 22 | 22 | DIY: correct only if built **without** a high-power E22 |
| rak11310 | 22 | 22 | |
| rak3112 | 22 | 22 | |
| rak4631 | 22 | 22 | without RAK13302 booster (see rak3401) |
| rak_wismesh_tag | 22 | 22 | |
| rpi_picow | 22 | 22 | |
| sensecap_solar | 22 | 22 | |
| tenstar_c3 (SX1262 / SX1268) | 22 | 22 | |
| thinknode_m1 / m2 / m5 / m6 | 22 | 22 | |
| waveshare_rp2040_lora | 22 | 22 | |
| wio-tracker-l1 / wio-tracker-l1-eink | 22 | 22 | |
| xiao_c3 / xiao_c6 (incl. Meshimi, WHY2025 badge) / xiao_nrf52 / xiao_rp2040 / xiao_s3 / xiao_s3_wio | 22 | 22 | Wio-SX1262 kit |

"No amplifier" means no PA, FEM, E22-M30S/M33S or booster appears anywhere in
the variant's files. `RXEN`/`TXEN` pins alone are antenna switches, not
amplifiers.

---

## 2. Setting = output, but the chip has quirks

### LR1110 — no external amplifier
The chip has two internal amplifiers, and RadioLib switches between them:
**≤ 14 → low-power amp** (efficient), **≥ 15 → high-power amp** (about 2× the
current).

| Board | Default | App max |
|---|---|---|
| t1000-e | 22 | 22 |
| minewsemi_me25ls01 | 22 | 22 |
| thinknode_m3 / m7 / m9 | 22 | 22 |
| wio_wm1110 | 22 | 22 |

The chip itself goes down to -17, but the companion firmware stops at -9.
Only the repeater/room-server `set tx` command reaches -17, and that value
resets to -9 on reboot.

### LR2021 — no external amplifier
| Board | Default | App max | Notes |
|---|---|---|---|
| meshtracker_x1 | 22 | 22 | sub-GHz range -9 … 22 (2.4 GHz would be -19 … 12) |

### STM32WL (SX126x inside the MCU) — no external amplifier
| Board | Default | App max | Notes |
|---|---|---|---|
| rak3x72 | 22 | 22 | HP + LP paths; the LP hardware version is max 14 |
| tiny_relay | 22 | 22 | HP + LP |
| wio-e5-dev / wio-e5-mini | 22 | 22 | HP path only |

### SX1276 (older chip) — **not every value works**
RadioLib (PA_BOOST path, which MeshCore uses) accepts only **2 … 17 and 20**.
**-9 … 1, 18 and 19 are rejected.** The radio keeps its previous power, but
the firmware still saves and reports the new number, so the app shows a value
the radio isn't using.

| Board | Default | App max |
|---|---|---|
| heltec_v2 | 20 | 20 |
| lilygo_t3s3_sx1276 | 20 | 20 |
| lilygo_tbeam_SX1276 | 20 | 20 |
| lilygo_tlora_v2_1 | 20 | 20 |

---

## 3. Setting ≠ output (external amplifier)

"≈ at antenna" uses the best source available. Unmeasured values are marked **~**.

| Board | Amplifier | Default → ≈ output | App max → ≈ output | Gain / source |
|---|---|---|---|---|
| **heltec_v4** (V4.2 GC1109 / V4.3 KCT8103L, auto-detected) | GC1109 or KCT8103L | 10 → ~21 dBm | 22 → **28.4–29 dBm** | +11 up to setting 15, falling to +7 at 22. Heltec measured table (meshtastic#8070), R&S meter (MeshCore#1708) |
| heltec_v4_r8 | KCT8103L | 10 → ~23 | 22 → ~29 | Meshtastic curve: +13 up to 14, falling to +7 at 22 |
| heltec_tracker_v2 | KCT8103L | 9 → ~22–23 | 22 → ~28–29 | firmware comment "+~13 dB"; Meshtastic curve +14 falling to +7 |
| heltec_t096 | KCT8103L | 9 → ~22–23 | 22 → ~28–29 | same as tracker_v2 |
| heltec_tower_v2 | KCT8103L (PA only) | 12 → ~22 | 22 → ~29 | firmware comment "~29 dBm"; Meshtastic +11 falling to +7 |
| lilygo_tbeam_1w | XY16P35 1 W module | 22 → ~32 | 22 → ~32 | firmware comment "+~10 dB → 32 dBm"; Meshtastic gain 10. Needs 4–8 V for full output |
| rak3401 ("1W Repeater") | RAK13302 booster, SKY66122 FEM | 22 → ~30 | 22 → ~30 | Meshtastic RAK13302 curve +7…+9 dB |
| gat562_30s_mesh_kit | 1 W PA (30S module) | 22 → ~29.5–30 | 22 → ~30 | vendor: 30 dBm max, "adjustable 5–30 dBm". MeshCore has no gain note |
| generic-e22 / meshadventurer | E22-900M30S (or 400M30S) | 22 → ~29–30 | 22 → ~30 | firmware comment "22 → ~30 dBm"; Meshtastic gain 7 (measured) |
| ikoka_*_nrf **e22_30dbm** builds (nano, stick, handheld) | E22-900M30S | 20 → ~27–28 | 20 → ~27–28 | max is 20 because MAX = default |
| ikoka_nano/stick **e22_33dbm** builds | E22-900M33S | 9 → ~33–34 | 9 → ~33 | Meshtastic gain 25 dB, capped at 8 there. Max 9 here protects the PA |
| station_g2 | Station G2 PA | 7 → ~27 | **19** → >30 (unmeasured) | firmware: "7 → ~27 dBm (~0.5 W)"; 19 = "max output without burning out the PA" (≈ +20 dB) |
| station_g3_esp32 | Station G3 PA (PL1/PL2 jumper levels) | 7 → depends on jumper | 22 → depends on jumper | output set by hardware PA level; Meshtastic caps at 19 |
| meshnology_w12 | LR2021 + GC1109 (no pad) | 4 → ~28–30 (unmeasured) | **4** | GC1109 saturates at 3–4 dBm input; max 4 protects it |

### Unknown — needs checking
| Board | Default = App max | Why it's flagged |
|---|---|---|
| lilygo_teth_elite (SX1262 shield) | 8 | Capped at 8 with no explanation, and no PA mentioned in the variant. Could be a high-power shield. Check LilyGo's shield spec |

### Not LoRa
generic_espnow and sensecap_indicator-espnow use ESP-NOW (Wi-Fi). "TX power"
there is Wi-Fi power: `esp_wifi_set_max_tx_power(dbm × 4)`.

---

## What this means for the app

- **Section 1 and LR1110/LR2021/STM32WL boards (the large majority):** the
  -9 … 22 field is literally dBm out of the radio. No change needed.
- **SX1276 boards:** limit the field to **2–17 and 20**. Otherwise the app
  shows a power the radio silently ignored.
- **PA boards:** the label "dBm" is misleading. Options:
  - show "≈ X dBm at antenna" next to the setting, using a per-board curve
    keyed on the device's reported model string;
  - or at least label it "radio drive level" on these boards.
- **Detecting the board:** the companion `DEVICE_INFO` model string (e.g.
  "Heltec V4 OLED" vs "Heltec V4.3 OLED") identifies most boards. The
  ikoka 22/30/33 dBm builds say so in `MANUFACTURER_STRING`.

## Open questions / to verify
- [ ] lilygo_teth_elite: why 8?
- [ ] station_g2 / station_g3: real output at 19–22 and per PA jumper level
- [ ] meshnology_w12: measured output at 4
- [ ] gat562_30s: is the PA a fixed gain, like the E22-M30S (≈ +8)?
- [ ] heltec_v4 V4.3 (KCT8103L): no measured table yet; Meshtastic uses the r8 curve (+13 …)
