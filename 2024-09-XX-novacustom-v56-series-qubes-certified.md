---
layout: post
title: "The NovaCustom V56 Series 16.0 inch coreboot laptop is Qubes certified!"
categories: announcements
image: /attachment/site/novacustom-v56-series.png
author: The NovaCustom and Qubes teams
---

It is our pleasure to announce that the [NovaCustom V56 Series 16.0 inch coreboot laptop](https://novacustom.com/product/v56-series/) is the eighth computer to be [officially certified](/doc/certified-hardware/) for Qubes OS Release 4 and the second such model from [NovaCustom](https://novacustom.com/)!

## V56 Series 16.0 inch coreboot laptop

Meet the world's most modern coreboot laptop. Thanks to an energy-efficient 14th generation Intel Meteor Lake processor, a powerful 73 WH battery, and a stunning 16:10 display, you'll be more productive than ever before. Configure this laptop with up to 96 GB of DDR5 memory and a blazing-fast PCIe SSD. Dasharo coreboot firmware provides you with a secure and stable base. Furthermore, this laptop features useful ports, including Thunderbolt 4, an Ethernet port, and plenty of USB ports. On top of that, this laptop is optionally available with Wi-Fi 7 support.

[![Photo of the NovaCustom V56 Series 16.0 inch coreboot laptop](/attachment/site/novacustom-v56-series.png)](https://novacustom.com/product/v56-series/)

## Qubes-certified options

The configuration options required for Qubes certification are detailed below.

### Screen size

- Certified: 16 inch model (V560TU)
- The 14-inch model (V540TU) is not currently certified.

### Screen resolution

- Certified: Full HD+ (1920 x 1200)
- Certified: Q-HD+ (2560 x 1600)

### Processor and graphics

- Certified: Intel Core Ultra 5 Processor 125H + Intel Arc iGPU with AI Boost
- Certified: Intel Core Ultra 7 Processor 155H + Intel Arc iGPU with AI Boost
- The Nvidia discrete GPU options are not currently certified.

### Memory

- Certified: Any configuration with at least 16 GB of memory

### Storage

- Certified: Any of the available options in this section

### Personalization

- This section is merely cosmetic and therefore does not affect certification.

### Firmware options

- Qubes OS does not currently support UEFI secure boot.
- Keeping up-to-date with firmware updates is merely an email notification service and therefore does not affect certification.
- Deploying coreboot+Heads does not affect certification, but it is not currently an available option for this model anyway.
- Disabling Intel Management Engine (HAP disabling) does not affect certification.

### Operating system

- Certified: Qubes OS 4.2.3 or newer (within Release 4).
- Releases older than 4.2.3 are not certified.
- You may choose either to have NovaCustom preinstall Qubes OS for you, or you may choose to install Qubes OS yourself. This choice does not affect certification.

### Wi-Fi and Bluetooth

- Certified: Intel AX-210/211 (non vPro) Wi-Fi module 2.4 Gbps, 802.11AX/Wi-Fi6E + Bluetooth 5.3
- Certified: Intel BE200 (non vPro) Wi-Fi module 5.8 Gbps, 802.11BE/Wi-Fi7 + Bluetooth 5.42
- Certified: No Wi-Fi chip - no Bluetooth and Wi-Fi connection possible (only with USB adapter)

## Disclaimers

- Currently requires `kernel-latest`: If you install Qubes OS yourself, you must select the `Install Qubes OS RX using kernel-latest` option on the GRUB menu when booting the installer. This non-default kernel option is currently required for the NovaCustom V56 Series to function properly.
- Due to a [known bug](https://github.com/Dasharo/dasharo-issues/issues/976), the bottom-right USB-C port is currently limited to USB 2.0 speeds.

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements](/doc/certified-hardware/#hardware-certification-requirements), and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](/doc/certified-hardware/#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
