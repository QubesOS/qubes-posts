---
layout: post
title: "The NovaCustom NV41 Series laptop is Qubes-certified!"
categories: announcements
image: /attachment/site/novacustom-nv41-series.png
---

It is our pleasure to announce that the [NovaCustom NV41 Series](https://novacustom.com/product/nv41-series/) laptop has become the fifth [Qubes-certified computer](/doc/certified-hardware/) for Qubes 4.X!

## About the NovaCustom NV41 Series

[![Photo of the NovaCustom NV41 Series](/attachment/site/novacustom-nv41-series.png)](https://novacustom.com/product/nv41-series/)

The [NV41 Series](https://novacustom.com/product/nv41-series/) is a 14-inch laptop from [NovaCustom](https://novacustom.com/), a European vendor known for their highly customizable, Linux-friendly laptops. This 12th Generation Intel Core (Alder Lake) laptop comes with Dasharo coreboot open-source firmware, USB-C charging, the latest Intel Xe graphics, and up to 64 GB of memory.

## Qubes-certified configurations

The following configuration options are certified for Qubes OS 4.X:

Processor:
- Intel Core i5-1240P processor
- Intel Core i7-1260P processor

Memory:
- 2 x 16 GB Kingston DDR4 SODIMM 3200 MHz (32 GB total)
- 1 x 32 GB Kingston DDR4 SODIMM 3200 MHz (32 GB total)
- 2 x 32 GB Kingston DDR4 SODIMM 3200 MHz (64 GB total)

M.2 storage chip:
- Samsung 980 SSD (all capacities)
- Samsung 980 Pro SSD (all capacities)

Wi-Fi and Bluetooth:
- Intel AX-200/201 Wi-Fi module 2976 Mbps, 802.11ax/Wi-Fi 6 + Bluetooth 5.2
- Killer (Intel) Wireless-AX 1675x M.2 Wi-Fi module 802.11ax/Wi-Fi 6E + Bluetooth 5.3
- Blob-free: Qualcomm Atheros QCNFA222 Wi-Fi 802.11a/b/g/n + Bluetooth 4.0
- No Wi-Fi/Bluetooth chip

### Notes on Wi-Fi and Bluetooth options

- When viewed in a Linux environment with `lspci`, the "Killer (Intel) Wireless-AX 1675x M.2 Wi-Fi module 802.11ax/Wi-Fi 6E + Bluetooth 5.3" device displays the model number "AX210." However, according to its [Intel Ark entry](https://ark.intel.com/content/www/us/en/ark/products/211485/intel-killer-wifi-6e-ax1675-xw.html) (in the "Product Brief" file), they are actually the same Wi-Fi module.

- Similarly, when viewed in a Linux environment with `lspci`, the "Blob-free: Qualcomm Atheros QCNFA222 Wi-Fi 802.11a/b/g/n + Bluetooth 4.0" device displays the model number "AR9462," which seems to be just the Wi-Fi chip model number, whereas "QCNFA222" seems to be the model number of the whole device (which include Bluetooth). Meanwhile, the Bluetooth device presents itself as "IMC Networks Device 3487."

- The term "blob-free" is used in different ways. In practice, being "blob-free" generally does *not* mean that the device does not use any closed-source firmware "blobs." Rather, it means that the device comes with firmware *preinstalled* so that it does not have to be loaded from the operating system. In theory, the preinstalled firmware could be open-source, but as far as we know, that is not the case with this particular Atheros Wi-Fi/Bluetooth module. (Qualcomm has published firmware source code in the past, but only for other device models, as far as we are aware.) Meanwhile, the Free Software Foundation (FSF) [considers](https://www.gnu.org/philosophy/free-hardware-designs.en.html#boundary) unmodifiable preinstalled firmware to be part of the hardware, hence they regard such hardware as "blob-free" from a software perspective. While common usage of the term "blob-free" often follows the FSF's interpretation, it is worthwhile for Qubes users who are concerned about closed-source firmware to understand the nuance.

## Special note regarding the need for `kernel-latest`

Beginning with Qubes OS 4.1.2, the Qubes installer includes the `kernel-latest` package and allows users to select this kernel option from the GRUB menu when booting the installer. At the time of this announcement, `kernel-latest` is **required** for the NovaCustom NV41 Series to function properly. Therefore, all potential purchasers and users of this model should be aware that they will have to select a non-default option (`Install Qubes OS RX using kernel-latest`) from the GRUB menu when booting the installer. However, since Linux 6.1 has officially been promoted to being a long-term support (LTS) kernel, it will become the default kernel at some point, which means that the need for this non-default selection is only temporary.

## What is Qubes-certified hardware?

[Qubes-certified hardware](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html#hardware-certification-requirements), and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
