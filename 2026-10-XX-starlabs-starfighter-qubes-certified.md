---
layout: post
title: "The Star Labs StarFighter is Qubes certified!"
categories: announcements
image: /attachment/site/starlabs-starfighter.png
author: The Star Labs and Qubes teams
---

It is our pleasure to announce that the [Star Labs StarFighter](https://starlabs.systems/pages/starfighter) is [officially certified](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html) for Qubes OS Release 4!

## The Star Labs StarFighter

The [Star Labs StarFighter](https://starlabs.systems/pages/starfighter) is a 16-inch laptop featuring open-source coreboot firmware, disabled Intel ME, up to 64 GB of memory, wireless kill switch, removable webcam, as well as extensive firmware controls for fan, power, security, boot behavior, and more.

[![Photo of Star Labs StarFighter](/attachment/site/starlabs-starfighter.png)](https://starlabs.systems/pages/starfighter)

## Qubes-certified options

The configuration options required for Qubes certification are detailed below.

### Base Configuration

- Certified: Standard (Intel Core Ultra 5 125H + 32GB LPDDR5X + QHD display)

- Certified: Ultra (Intel Core Ultra 9 285H + 64GB LPDDR5X + 4K display)

- The AMD configuration is not certified.

- **Warning:** While both the QHD and 4K display variants will work, the 4K display will run at a lower resolution (Full HD) due to the absence of proper support for HiDPI in Qubes OS. While users can manually change the resolution to 4K, we cannot guarantee everything will work properly. For example, there are known issues with both UI scaling and full-screen video playback performance in 4K. This limitation may be lifted in a future Qubes OS release.

### Operating System

- Certified: Qubes OS 4.3.1 or newer (within Release 4).

- Releases older than 4.3.1 are not certified.

- You may choose either to have Star Labs preinstall Qubes OS for you, or you may choose to install Qubes OS yourself. This choice does not affect certification.

### Storage & Additional Storage

- Certified: All of the available options in these sections

## Disclaimers

- As mentioned above, while both the QHD and 4K display variants will work, the 4K display will run at a lower resolution (Full HD) due to the absence of proper support for HiDPI in Qubes OS. While users can manually change the resolution to 4K, we cannot guarantee everything will work properly. For example, there are known issues with both UI scaling and full-screen video playback performance in 4K. This limitation may be lifted in a future Qubes OS release.

## What is Qubes-certified hardware?

[Qubes-certified hardware](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html#hardware-certification-requirements), and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](https://doc.qubes-os.org/en/latest/user/hardware/certified-hardware/certified-hardware.html#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
