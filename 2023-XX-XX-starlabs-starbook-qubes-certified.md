---
layout: post
title: "The Star Labs StarBook is Qubes-certified!"
categories: announcements
image: /attachment/site/starlabs-starbook.png
author: The Star Labs and Qubes teams
---

It is our pleasure to announce that the [Star Labs StarBook](https://starlabs.systems/pages/starbook) is [officially certified](/doc/certified-hardware/) for Qubes OS Release 4!

## The Star Labs StarBook

The [Star Labs StarBook](https://starlabs.systems/pages/starbook) is a 14-inch laptop featuring open-source coreboot and EDK II firmware. In addition, the StarBook is currently the *only* Qubes-certified computer with out-of-the-box support for `qubes-fwupdmgr`, a new feature in Qubes OS 4.2 that allows Qubes OS to securely update the computer's firmware.

[![Photo of Star Labs StarBook](/attachment/site/starlabs-starbook.png)](https://starlabs.systems/pages/starbook)

The Qubes developers have tested and certified the following StarBook configuration options for Qubes OS 4.X:

| Component        | Qubes-certified options                          |
| ---------------- | ------------------------------------------------ |
| Processor        | 13th Generation Intel Core i3-1315U or i7-1360P  |
| Memory           | 8 GB, 16 GB, 32 GB, or 64 GB RAM                 |
| Storage          | 512 GB, 1 TB, or 2 TB SSD                        |
| Graphics         | Intel (integrated graphics)                      |
| Networking       | Intel Wi-Fi 6 AX210 (no built-in wired Ethernet) |
| Firmware         | coreboot 8.97 (2023-10-03)                       |
| Operating system | Qubes OS (pre-installation optional)             |

[![Photo of Star Labs StarBook](/attachment/site/starlabs-starbook_top.png)](https://starlabs.systems/pages/starbook)

The StarBook features a true matte 14-inch IPS display at 1920x1080 full HD resolution with 400cd/m² of brightness, 178° viewing angles, and a 180° hinge. The backlit keyboard is available in US English, UK English, French, German, Nordic, and Spanish layouts.

[![Photo of Star Labs StarBook](/attachment/site/starlabs-starbook_side.png)](https://starlabs.systems/pages/starbook)

The StarBook includes four USB ports (1x USB-C with Thunderbolt 4, 2x USB 3.0, and 1x USB 2.0), one HDMI port, a microSD slot, an audio input/output combo jack, and a DC jack for charging. For more information, see the official [Star Labs StarBook](https://starlabs.systems/pages/starbook) page.

[![Photo of Star Labs StarBook](/attachment/site/starlabs-starbook_back.png)](https://starlabs.systems/pages/starbook)

## Special note regarding the need for `kernel-latest` on Qubes OS 4.1

Beginning with Qubes OS 4.1.2, the Qubes installer includes the `kernel-latest` package and allows users to select this kernel option from the GRUB menu when booting the installer. If you plan to install Qubes OS 4.1 on the StarBook, please be aware that you will have to select this non-default option. By contrast, Qubes OS 4.2 is confirmed to work with the default kernel option and does not require `kernel-latest`.

## About Star Labs

In short, we're just a bunch of geeks. Back in 2016, Star Labs was formed in a pub. We all depended on using Linux, all with different laptops and all with different complaints about them. It always perplexed us that a laptop had never been made specifically for Linux. Whilst many had been "converted" to run Linux - they seldom offered the experience that macOS and Windows users had. So, after a few pints, we decided to make one. [Read the full story on the Star Labs website.](https://us.starlabs.systems/pages/about-us)

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements], and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](/doc/certified-hardware/#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
