---
title: Heltec V4 Transmit Power
date: 2026-04-10
draft: false
---
Heltec V4 output power settings.

| TX Setting | Actual Output (dBm) | Current Draw (mA) | Notes |
| :--- | :--- | :--- | :--- |
| **16** | +25.34 dBm | 468 mA | Good for low-power nodes |
| **17** | +27.14 dBm | 589 mA | High-efficiency threshold |
| **18** | +27.31 dBm | 605 mA | **The "Sweet Spot"** |
| **19** | +27.58 dBm | 637 mA | Marginal gains |
| **20** | +27.74 dBm | 709 mA | Practical maximum |
| **22** | +28.44 dBm | 741 mA | Hard firmware limit |

### Recommended TX Power Setting

For the best balance of range and battery life, the recommended setting is **TX 18**.

---

**Credits & Source:**
This data was compiled from community benchmarks performed by **JasonKrijgsman** and calibrated measurements by **towerviewcams**. 

For the full technical discussion and raw data, see the original GitHub issue: 
[MeshCore Issue #1708: Heltec V4 Measured TX Power Output Table](https://github.com/meshcore-dev/MeshCore/issues/1708)
