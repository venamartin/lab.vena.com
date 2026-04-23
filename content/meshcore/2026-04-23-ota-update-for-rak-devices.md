---
title: OTA Update for RAK Devices
date: 2026-04-23
tags:
  - meshcore
  - ota
author: Martin
draft: false
summary: Instructions for Over the Air (OTA) updates for RAK, T114 MeshCore
  repeaters and room servers utilizing the nRF DFU app for iOS and Android.
---
These steps are compatible with both Android and iOS, as the nRF DFU app interface is identical on both platforms.

> [!Note]
> These instructions were summarized from: [meshcore.io](https://docs.meshcore.io/faq/#7-other-questions)

### 1. Prerequisites

- Download the **nRF Device Firmware Update** (nRF DFU) app from the [iOS App Store](https://apps.apple.com/app/nrf-device-firmware-update/id1624454838) or [Google Play Store](https://play.google.com/store/apps/details?id=no.nordicsemi.android.dfu).
- Visit [flasher.meshcore.io](https://flasher.meshcore.io) and download the **ZIP version** of the firmware specific to your device (e.g., RAK, Heltec T114, or Seeed Studio Xiao).

### 2. Prepare the Device

1. Open the MeshCore app and log in remotely to the repeater you wish to update (requires admin privileges).
2. Navigate to the **Command Line** tab.
3. Enter the following command:
  ```bash
    start ota
  ```
4. Confirm that the response says `OK`. The device is now in OTA mode.

### 3. Configure the nRF DFU App

1. Open the nRF DFU app and tap **Settings** in the top right corner.
2. Enable **Packets receipt notifications**.
3. Change the **Number of Packets** based on your hardware:
  - **RAK:** 10 (or 8)
  - **T114:** 8
4. Select the firmware ZIP file you downloaded earlier.

### 4. Perform the Update

1. Select the device you want to update from the list.
  - *Note: If the device is not found, try enabling `start ota` again or enable **Force Scanning** in the DFU app settings.*
2. Tap **Upload** to begin the process.
3. Wait for the update to complete. This may take several minutes.

### Troubleshooting

- **Connection issues:** Toggle Bluetooth off and back on, or reboot your phone.
- **Bootloader Errors:** If the update fails at the "Enabling Bootloader" step, "Forget" the nRF board in your phone's Bluetooth settings and re-pair it directly through the DFU app.

