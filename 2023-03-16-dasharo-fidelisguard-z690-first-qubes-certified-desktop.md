---
layout: post
title: "The Dasharo FidelisGuard Z690 is the first Qubes-certified desktop computer!"
categories: announcements
image: /attachment/posts/dasharo-fidelisguard-z690_2.jpg
---

It is our pleasure to announce that the [Dasharo FidelisGuard Z690](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/) has become the fourth [Qubes-certified computer](/doc/certified-hardware/) for Qubes 4.X and the **first** Qubes-certified desktop computer **ever**!

## About the Dasharo FidelisGuard Z690

[![Photo of MSI PRO Z690-A DDR4 motherboard](/attachment/posts/dasharo-fidelisguard-z690_1.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/)

The [Dasharo FidelisGuard Z690](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/) is a full desktop PC build that brings the [Dasharo](https://dasharo.com/) open-source firmware distribution to the MSI PRO Z690-A DDR4 motherboard with Qubes OS preinstalled. The full configuration includes:

| Part         | Model Name                                                     |
|------------- | -------------------------------------------------------------- |
| CPU	         | Intel Core i5-12600K, 3.7GHz                                   |
| Cooling	     | Noctua CPU NH-U12S Redux                                       |
| RAM	         | Kingston Fury Beast, DDR4, 4x8GB (32 GB Total), 3600 MHz, CL17 |
| Power Supply | Seasonic Focus PX 750W 80 Plus Platinum                        |
| Storage      | SSD Intel 670p 512 GB M.2 2280 PCI-E x4 Gen3 NVMe              |
| Enclosure	   | SilentiumPC Armis AR1                                          |

[![Photo of Dasharo FidelisGuard Z690 with open case](/attachment/posts/dasharo-fidelisguard-z690_2.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/)

This computer comes with a "Dasharo Supporters Entrance Subscription," which includes the following:

- Full access to [Dasharo Tools Suite (DTS)](https://docs.dasharo.com/dasharo-tools-suite/overview/)
- The latest Dasharo releases issued by the Dasharo Team
- Special Dasharo updates for supporters
- Dasharo Premier Support through an invite-only Matrix channel
- Influence on the Dasharo feature roadmap

[![Photo of Dasharo FidelisGuard Z690 with open case](/attachment/posts/dasharo-fidelisguard-z690_3.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/)

For further details, please see the [Dasharo FidelisGuard Z690](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/) product page.

[![Photo of the outside of the Dasharo FidelisGuard Z690](/attachment/posts/dasharo-fidelisguard-z690_4.jpg)](https://3mdeb.com/shop/open-source-hardware/dasharo-fidelisguard-z690-qubes-os-certified/)

## Special note regarding the need for `kernel-latest`

Beginning with Qubes OS 4.1.2, the Qubes installer includes the `kernel-latest` package and allows users to select this kernel option from the GRUB menu when booting the installer. At the time of this announcement, `kernel-latest` is **required** for the Dasharo FidelisGuard Z690's graphics drivers to function properly. Therefore, all potential purchasers and users of this model should be aware that they will have to select a non-default option (`Install Qubes OS RX using kernel-latest`) from the GRUB menu when booting the installer. However, since Linux 6.1 has officially been promoted to being a long-term support (LTS) kernel, it will become the default kernel at some point, which means that the need for this non-default selection is only temporary.

## About Dasharo

"Dasharo is an open-source firmware distribution focusing on seamless deployment, clean and simple code, long-term maintenance, professional support, transparent validation, superior documentation, privacy-respecting implementation, liberty for the owners and trustworthiness for all." [Learn more about Dasharo.](https://docs.dasharo.com/osf-trivia-list/dasharo/)

Dasharo is a registered trademark of and a product developed by [3mdeb](https://3mdeb.com/).

## About 3mdeb

3mdeb and the Qubes OS Project have been partnering together for years to hold Qubes OS Summits. Michał Żygowski shared the story with us in [Qubes OS Summit: History from organizer's perspective](/news/2022/09/07/qubes-os-summit-history/). You can watch videos from the 2022 summit [here](https://www.youtube.com/watch?v=hkWWz3xGqS8) and [here](https://www.youtube.com/watch?v=A9GrlQsQc7Q). 3mdeb has also been instrumental in recent work on [TrenchBoot Anti Evil Maid for Qubes OS](/news/2023/01/31/trenchboot-aem-for-qubes-os/). Learn more about 3mdeb [here](https://3mdeb.com/about-us/).

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements], and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](/doc/certified-hardware/#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
