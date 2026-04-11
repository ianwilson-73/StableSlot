# Licensing

This project is licensed under Apache License 2.0.

## Commercial Rights
The author retains full rights to commercialise this project.

Commercial licenses or partnerships are available on request.

## Hardware Notes
Reference designs are open. Production variants may remain proprietary.

## Trademarks

StableSlot™ is the project name and brand of Ian Wilson.
The source code is licensed under Apache-2.0, but the project name,
logo, and branding are not licensed except as allowed under trademark law
and Apache License 2.0 section 6.

## Disclaimer
Provided as-is, please see the seperate DISCLAIMER.md file.

# StableSlot™ Note:

* **Relates to:** ResMed Series 10 and 11
  
* **Hardware:** [SD WIFI PRO](https://www.fysetc.com/products/fysetc-upgrade-sd-wifi-pro-with-card-reader-module-run-wireless-by-esp32-chip-web-server-reader-uploader-3d-printer-parts) — an ESP32-powered SD card that physically inserts into your CPAP's SD card slot like a regular memory card - combined with

* **Software:** [CPAP Auto-Sync](https://github.com/ilyakruchinin/CPAP-AutoSync) — ESP32 firmware that uploads CPAP data to SMB and SleepHQ services

**OR**

* **Alternate Hardware:** [SLEEPHQ Magic Uploader](https://shop.sleephq.com/products/magic-uploader-pro?srsltid=AfmBOoqpUE9kaXoKS9YDrxQcV_XHUk0DoiBh5K879EuiGGwW1DfBCF8M) — a proprietary upload solution that uses an EZ Share SD WIFI card and bespoke Raspberry Pi package.



* May be relevant to other SD Wifi card solutions used with these CPAP machines
---

## Explanation

SD WIFI cards require far more power than a simple, traditional, SD storage card. CPAP machines were built only with these simple storage only SD devices in mind, and so when it came to designing power supplies for the SD interface, the need for a little extra power perhaps being needed was never part of the design thought process.

**⚠️ Identifying Power Issues**

If your CPAP cannot provide enough power to the SD WIFI card, the SD card chip will reset itself. You might notice:
 - The CPAP machine complains of SD Card Errors
 - The device disappears from WiFi frequently
 - Uploads fail midway or never start
 - Any web interface is unreliable

This happens when there is a surge in power requirement, for example when wifi transmissions take place, and the power supply cannot keep up. The voltage will drop, and once below a critical level, then trouble will begin.

## The Fix

Following experimentation, I found that surges in power demand could be protected against by inserting a capacitor in place between two pins of the SD card. Various types and strengths of capacitor were tried in order to find a range within which stability could be guaranteed, but without over-stressing the power supply to the SD WIFI card.

## StableSlot™ Was Born

I was actually dealing with SD WIFI card (of another type) stability issues with a CPAP machine. While researching the cause and trying to find a solution, I happened to come across [CPAP AutoSync by Ilya Kruchinin](https://github.com/ilyakruchinin/CPAP-AutoSync). Ilya has produced an amazing piece of software that he got running on the ESP32 platform. Even more amazingly, it turns out that you can get an SD WIFI card with an ESP32 on board, and so Ilya has developed a piece of software that runs directly on this SD WIFI card, autonomously uploading the user's CPAP data to the cloud (SleepHQ) and SMB network storage devices.

I was so impressed and inspired by Ilya's creation that I soon ordered one of the required [SD WIFI PRO Cards](https://www.fysetc.com/products/fysetc-upgrade-sd-wifi-pro-with-card-reader-module-run-wireless-by-esp32-chip-web-server-reader-uploader-3d-printer-parts) and I began to test out the software. This was when I started to run into the same stability issues I had seen with the other brand of SD WIFI card on testing. After careful testing and reading, I managed to develop the cure to the issues a number of users had by then been plagued with.

Following further development, an enclosure was produced that houses the electronics, SD WIFI card and that provides an SD card format connected to insert into the CPAP machine. I was thinking about producing these as a kit, but as I started to talk to people, it started to become apparent that not everybody feels comfortable messing around with tiny surface mounted circuit boards and 3D printing enclosures for their project.

I therefore went on to produce a device that could be bought by a user to help overcome any issues of their SD WIFI card instability. These devices can be purchased directly from us, and will be shipped out ready to use.

## What is StableSlot™

In it's purchased form, StableSlot™ is a little unit that you simply plug into your CPAP machine's SD card slot. The unit comes ready prepared, and has the latest version of CPAP AutoSync already on it and ready to go. All the user needs to do is plug the device in, connect to it on any web browser, and set up their credentials and WIFI details. The unit will then restart, join your normal WIFI network, and autonomously upload the user's CPAP data as they set up at the end of each therapy session.

Full instructions will be supplied with each device, but it really is simple to use and set up.

## Where do I get StableSlot™

StableSlot™ is available for purchase from our [Square Store](https://)
