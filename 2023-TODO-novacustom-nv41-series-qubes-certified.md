---
layout: post
title: "The NovaCustom NV41 Series laptop is Qubes-certified!"
categories: announcements
image: /attachment/site/novacustom-nv41-series.png
---

It is our pleasure to announce that the [NovaCustom NV41 Series](https://configurelaptop.eu/nv41-series/) laptop has become the fifth [Qubes-certified computer](/doc/certified-hardware/) for Qubes 4.X!

## About the NovaCustom NV41 Series

[![Photo of the NovaCustom NV41 Series](/attachment/site/novacustom-nv41-series.png)](https://configurelaptop.eu/nv41-series/)

The [NV41 Series](https://configurelaptop.eu/nv41-series/) is a 14-inch laptop from [NovaCustom](https://configurelaptop.eu/), a European vendor known for their highly customizable, Linux-friendly laptops. This 12th-generation Intel Core (Alder Lake) laptop comes with Dasharo coreboot open-source firmware, USB-C charging, the latest Intel Xe graphics, and up to 64 GB of memory.

## Qubes-certified configurations

The following configuration options are certified for Qubes OS 4.X:

Processor:
- Intel Core i5-1240P
- Intel Core i7-1260P

Memory (Dual Channel):
- 2 x 16 GB (32 GB total)
- 1 x 32 GB (32 GB total)
- 2 x 32 GB (64 GB total)

Storage:
- Samsung 980 (all capacities)
- Samsung 980 Pro (all capacities)

Wi-Fi:
- Intel AX-200/201
- Killer (Intel) Wireless-AX 1675x
- No Wi-Fi

### Notes

- The blob-free Qualcomm Atheros QCNFA222 Wi-Fi module is still in testing.
- When viewed in a Linux environment, the Killer (Intel) Wireless-AX 1675x M.2 Wi-Fi module displays the model number "AX210." However, according to its [Intel Ark entry](https://ark.intel.com/content/www/us/en/ark/products/211485/intel-killer-wifi-6e-ax1675-xw.html) (see the "Product Brief" file), they are actually the same Wi-Fi module.

## Special note regarding the need for `kernel-latest`

Beginning with Qubes OS 4.1.2, the Qubes installer includes the `kernel-latest` package and allows users to select this kernel option from the GRUB menu when booting the installer. At the time of this announcement, `kernel-latest` is **required** for the NovaCustom NV41 Series to function properly. Therefore, all potential purchasers and users of this model should be aware that they will have to select a non-default option (`Install Qubes OS RX using kernel-latest`) from the GRUB menu when booting the installer. However, since Linux 6.1 has officially been promoted to being a long-term support (LTS) kernel, it will become the default kernel at some point, which means that the need for this non-default selection is only temporary.

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements], and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](/doc/certified-hardware/#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
