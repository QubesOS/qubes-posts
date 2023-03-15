---
layout: post
title: "The 3mdeb MSI PRO Z690-A DDR4 is the first Qubes-certified desktop computer!"
categories: announcements
image: /attachment/posts/3mdeb-msi-pro-z690-a-ddr4_2.jpg
---

It is our pleasure to announce that the [3mdeb MSI PRO Z690-A DDR4](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/) has become the fourth [Qubes-certified computer](/doc/certified-hardware/) for Qubes 4.X and the **first** Qubes-certified desktop computer **ever**!

## About the 3mdeb MSI PRO Z690-A DDR4

[![Photo of MSI PRO Z690-A DDR4 motherboard](/attachment/posts/3mdeb-msi-pro-z690-a-ddr4_1.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/)

The [3mdeb MSI PRO Z690-A DDR4](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/) is a full desktop PC build that brings the [Dasharo](https://dasharo.com/) open-source firmware distribution to the MSI PRO Z690-A DDR4 motherboard. The full configuration includes:

| Part         | Model Name                                                      |
|------------- | --------------------------------------------------------------- |
| CPU	         | Intel Core i5-12600K, 3.7GHz                                    |
| Cooling	     | Noctua CPU NH-U12S Redux (w/ Noctua NM-i17xx-MP78 Mounting Kit) |
| RAM	         | Kingston Fury Beast, DDR4, 4x8GB (32 GB Total), 3600 MHz, CL17  |
| Power Supply | Seasonic Focus PX 750W 80 Plus Platinum                         |
| Storage      | SSD Intel 670p 512 GB M.2 2280 PCI-E x4 Gen3 NVMe               |
| Enclosure	   | SilentiumPC Armis AR1                                           |

[![Photo of 3mdeb MSI PRO Z690-A DDR4 with open case](/attachment/posts/3mdeb-msi-pro-z690-a-ddr4_2.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/)

As 3mdeb explains, "The integral part of this product is [Dasharo Tools Suite Supporters Entrance](https://docs.dasharo.com/osf-trivia-list/dts/#what-is-dasharo-tools-suite-supporters-entrance)," or "DTS SE," which provides access to Dasharo firmware updates for your hardware. This includes:

- The latest Dasharo release installed by the Dasharo Team
- Dasharo updates, where the number of updates depends on the number of Dasharo subscriptions sold and the availability of other funding (e.g., NLNet, corporate sponsors, community donations)
- Priority support through an invite-only Matrix channel
- Influence on the Dasharo feature roadmap

3mdeb points out that, by purchasing the MSI PRO Z690-A DDR4 with DST SE, you can have a meaningful impact on the course of Dasharo's development, as well as support the development of open-source firmware and the Dasharo distribution itself.

[![Photo of 3mdeb MSI PRO Z690-A DDR4 with open case](/attachment/posts/3mdeb-msi-pro-z690-a-ddr4_3.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/)

For further details, please see the [MSI PRO Z690-A DDR4](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/) product page.

[![Photo of the outside of the 3mdeb MSI PRO Z690-A DDR4](/attachment/posts/3mdeb-msi-pro-z690-a-ddr4_4.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-compatible-with-msi-pro-z-690a-ddr4-full-pc-build/)

## Special note regarding the need for `kernel-latest`

Beginning with Qubes OS 4.1.2, the Qubes installer includes the `kernel-latest` package and allows users to select this kernel option from the GRUB menu when booting the installer. At the time of this announcement, `kernel-latest` is **required** for the MSI PRO Z690-A DDR4's graphics drivers to function properly. Therefore, all potential purchasers and users of this model should be aware that they will have to select a non-default option (`Install Qubes OS RX using kernel-latest`) from the GRUB menu when booting the installer. However, since Linux 6.1 has officially been promoted to being a long-term support (LTS) kernel, it will become the default kernel at some point, which means that the need for this non-default selection is only temporary.

## About 3mdeb

3mdeb and the Qubes OS Project have been partnering together for years to hold Qubes OS Summits. Michał Żygowski shared the story with us in [Qubes OS Summit: History from organizer's perspective](/news/2022/09/07/qubes-os-summit-history/). You can watch videos from the 2022 summit [here](https://www.youtube.com/watch?v=hkWWz3xGqS8) and [here](https://www.youtube.com/watch?v=A9GrlQsQc7Q). 3mdeb has also been instrumental in recent work on [TrenchBoot Anti Evil Maid for Qubes OS](/news/2023/01/31/trenchboot-aem-for-qubes-os/). Learn more about 3mdeb [here](https://3mdeb.com/about-us/).

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements], and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified laptops](/doc/certified-hardware/#qubes-certified-laptops), in particular, are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
