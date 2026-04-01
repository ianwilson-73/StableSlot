# Licensing

This project is licensed under Apache License 2.0.

## Commercial Rights
The author retains full rights to commercialise this project.

Commercial licenses or partnerships are available on request.

## Hardware Notes
Reference designs are open. Production variants may remain proprietary.

## Repo Structure
/open-core
/hardware-ref
/production (private)
/cloud (private)

## Trademarks
Not licensed.

## Disclaimer
Provided as-is.

# AIRSENSE 10/11 SD WIFI CARD BROWNOUT PREVENTION NOTES

Trying to make sense of and prevent SD WIFI CARD issues in AS10/11 machines**

* **Relates to:** ResMed Series 10 and 11
  
* **Hardware:** [SD WIFI PRO](https://www.fysetc.com/products/fysetc-upgrade-sd-wifi-pro-with-card-reader-module-run-wireless-by-esp32-chip-web-server-reader-uploader-3d-printer-parts) — an ESP32-powered SD card that physically inserts into your CPAP's SD card slot like a regular memory card

* **Software:** [CPAP Auto-Sync](https://github.com/ilyakruchinin/CPAP-AutoSync) — ESP32 firmware that uploads CPAP data to SMB and SleepHQ services

* May be relevant to other SD Wifi card solutions used with these CPAP machines
---

## ⚠️ **IMPORTANT COMPATIBILITY NOTICE**

### ** Known Power Compatibility & Known Hardware Limits**

> [!NOTE]
> ℹ️ **AirSense 10 units:** These machines power-cycle the SD card slot every 60 seconds while actively blowing air. This causes the ESP32 card to constantly reboot during therapy, which will degrade the Web UI experience while you are sleeping. However, **this does not affect functionality** — once you stop therapy (take off the mask or stop the machine from blowing air), the card will boot up normally and complete the upload as expected.

> [!CAUTION]
> ⚠️ **AirSense 11** ***(🔍 ONLY REF 39517, check back sticker! 🏷️)*** ➔ Most **REF 39517** units have severe power limitations on their SD card slot. If the ESP32 card does not receive enough power, it will continually reset. You may experience frequent WiFi disconnects, failed uploads, or an "**SD Card Error**" on your CPAP machine's screen.

We are currently gathering statistics on which models work reliably. **If your model is not listed below, please report your experience to help us improve this data.**

**👇👇👇 Click to expand:**
<details>
<summary>
  <img src="./docs/logo/animated-arrow.svg?v3" alt="Point" width="25" style="vertical-align: middle;"/> 
  <b style="font-size: 1.2em; vertical-align: middle;">Detailed Model Compatibility Statistics</b>
</summary>

| Model | Made In | Platform | REF | Modem | Success rate | Notes |
| :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **AirSense 11** | Singapore | `R390-420/1` | 39480 | *(not specified / Europe)* | ✅ **100%** | Fully working |
| **AirSense 11** | Singapore | `R390-451/1` | 39483 | *(not specified / Europe)* | ✅ **100%** | Fully working |
| **AirSense 11** | Singapore | `R390-447/1` | 39517 | AIR11M1G22 | ❌ **35%** | Has known power delivery issues. Fails on most units. |
| ↳ *(modded)* | — | — | ↳ 39517 🔧 | — | ⚠️ **65%** | *With 1uF SD Extender Mod and `BROWNOUT_DETECT=OFF`* |
| **AirSense 11** | Singapore | `R390-447/1` | 39520 | AIR11M1G22 | ✅ **100%** | Fully working |
| **AirSense 11** | Singapore | `R390-447/1` | 39523 | AIR11M1U | ✅ **100%** | Stable since v1.0i-beta1 (had issues prior) |
| **AirSense 11** | Australia | `R390-453/1` | 39532 | AIR114GT | ✅ **100%** | Fully working |
| **AirSense 10** | Australia | `R370-4102/1` | 37043 | AIR104G | ✅ **100%** | ℹ️ Fully working, see notes |
| **AirSense 10** | Singapore | `R370-4201/1` | 37127 | *(not specified / Europe)* | ✅ **100%** | ℹ️ Fully working, see notes |
| **AirSense 10** | Singapore | `R370-4207/1` | 37160 | AIR104GU | ✅ **100%** | ℹ️ Fully working, see notes |
| **AirSense 10** | Australia | `R370-449/1` | 37437 | *(not specified / Australia)* | ✅ **100%** | ℹ️ Fully working, see notes |

> 💡 **TIP: Hardware Modification Work in Progress**
> 
> One of our users is currently testing an **SD Card Extender mod** to add more capacitance to the power line. Initial tests show promising results (improving the R390-447/1 REF 39517 from 35% to 65% success rate). We are waiting for further testing with increased capacitance, which may fully resolve power issues for the problematic models. Investigations are also ongoing to see if a capacitor mod (or a newer AirSense firmware) might resolve the mid-therapy reboot issue on AirSense 10 units.

</details>

---

<details>
<summary><b>🔍 How to tell if your CPAP has power issues</b></summary>

> **⚠️ Identifying Power Issues**
>
> If your CPAP cannot provide enough power to the SD card, the ESP32 chip will reset itself. You might notice:
> - The device disappears from WiFi frequently
> - Uploads fail midway or never start
> - The web interface is unreliable
>
> You can confirm this is happening by looking at your logs:
> 1. If `PERSISTENT_LOGS=true` is set, check the downloaded logs from the web interface.
> 2. If the device cannot even stay online long enough to broadcast WiFi, pull the SD card and look for a file called `uploader_error.txt`.
>
> Look for this specific warning:
> ```text
> [INFO] Reset reason: Brown-out reset (low voltage)
> [ERROR] WARNING: System reset due to brown-out (insufficient power supply), this could be caused by:
> [ERROR]  - the CPAP was disconnected from the power supply
> [ERROR]  - the card was removed
> [ERROR]  - the CPAP machine cannot provide enough power
> ```

> **Versions between v0.11.0 and v3.0i:** Added progressively more aggressive power optimizations (reduced TX power, 802.11b disabled, Bluetooth disabled, CPU throttled, WiFi modem-sleep enabled) specifically to improve AirSense 11 compatibility, which allowed some previously incompatible models to work. Firmware configurations like `BROWNOUT_DETECT=OFF` can also help on borderline machines.
</details>
