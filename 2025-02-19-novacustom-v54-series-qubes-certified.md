---
layout: post
title: "The NovaCustom V54 Series 14.0 inch coreboot laptop is Qubes certified!"
categories: announcements
image: /attachment/site/novacustom-v54-series.png
author: The NovaCustom and Qubes teams
---

It is our pleasure to announce that the [NovaCustom V54 Series 14.0 inch coreboot laptop](https://novacustom.com/product/v54-series/) is [officially certified](/doc/certified-hardware/) for Qubes OS Release 4!

## V54 Series 14.0 inch coreboot laptop

Introducing the latest coreboot laptop equipped with cutting-edge technology. With an ultra-efficient 14th-gen Intel Meteor Lake CPU, a robust 73 WH battery, and a captivating 16:10 screen, your productivity will soar to new heights. Customize your device with a maximum of 96 GB DDR5 RAM and up to two lightning-fast PCIe SSDs. The Dasharo coreboot firmware ensures a reliable and secure foundation. Additionally, enjoy a variety of connectivity options such as Thunderbolt 4, Ethernet, plentiful USB ports, and optional Wi-Fi 7.

[![Photo of the NovaCustom V54 Series 14.0 inch coreboot laptop](/attachment/site/novacustom-v54-series.png)](https://novacustom.com/product/v54-series/)

## Qubes-certified options

The configuration options required for Qubes certification are detailed below.

### Screen size

- Certified: 14 inch

**Note:** The 14-inch model (V540TU) and the 16-inch model (V560TU) are two separate products. [The 16-inch model has already been certified.](/news/2024/09/17/novacustom-v56-series-qubes-certified/)

### Screen resolution

- Certified: Full HD+ (1920 x 1200)
- Certified: 2.8K (2880 x 1800)

### Processor and graphics

- Certified: Intel Core Ultra 5 Processor 125H, Intel Arc iGPU with AI Boost
- Certified: Intel Core Ultra 7 Processor 155H, Intel Arc iGPU with AI Boost
- The Nvidia discrete GPU options are not currently certified.

### Memory

- Certified: Any configuration with at least 16 GB of memory

### Storage

- Certified: All of the available options in these sections

### Personalization

- This section is merely cosmetic and therefore does not affect certification.

### Firmware options

- Qubes OS does not currently support UEFI secure boot.
- The option to be kept up to date with firmware updates is merely an email notification service and therefore does not affect certification.
- The coreboot+Heads option is not currently certified. This option is a separate firmware variant. As such, it requires a separate certification process, which we expect to occur in the future.
- Disabling Intel Management Engine (HAP disabling) does not affect certification.

### Operating system

- Certified: Qubes OS 4.2.4 or newer (within Release 4).
- Releases older than 4.2.4 are not certified.
- You may choose either to have NovaCustom preinstall Qubes OS for you, or you may choose to install Qubes OS yourself. This choice does not affect certification.

### Wi-Fi and Bluetooth

- Certified: Intel AX-210/211 (non vPro) Wi-Fi module 2.4 Gbps, 802.11AX/Wi-Fi6E + Bluetooth 5.3
- Certified: Intel BE200 (non vPro) Wi-Fi module 5.8 Gbps, 802.11BE/Wi-Fi7 + Bluetooth 5.42
- Certified: No Wi-Fi chip -- no Bluetooth and Wi-Fi connection possible (only with USB adapter)

## Disclaimers

- In order for Wi-Fi to function properly, `sys-net` must currently be based on a Fedora template. The firmware package in Debian templates is currently too old for the certified Wi-Fi cards.
- Currently requires `kernel-latest`: If you install Qubes OS yourself, you must select the `Install Qubes OS RX using kernel-latest` option on the GRUB menu when booting the installer. This non-default kernel option is currently required for the NovaCustom V54 Series to function properly.
- Due to a [known bug](https://github.com/Dasharo/dasharo-issues/issues/976), the bottom-right USB-C port is currently limited to USB 2.0 speeds.

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements](/doc/certified-hardware/#hardware-certification-requirements), and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](/doc/certified-hardware/#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
