---
layout: post
title: "The NitroPC Pro is Qubes-certified!"
categories: announcements
image: /attachment/posts/nitropc-pro.jpg
author: The Nitrokey and Qubes teams
---

It is our pleasure to announce that the [NitroPC Pro](https://shop.nitrokey.com/shop/product/nitropc-pro-523) is [officially certified](/doc/certified-hardware/) for Qubes OS Release 4!

## The NitroPC Pro: a secure, powerful workstation

The [NitroPC Pro](https://shop.nitrokey.com/shop/product/nitropc-pro-523) is a workstation for high security and performance requirements. The open-source [Dasharo coreboot](https://github.com/Dasharo/coreboot) firmware ensures high transparency and security while avoiding backdoors and security holes in the firmware. The device is certified for compatibility with Qubes OS 4.X by the Qubes developers. Carefully selected components ensure high performance, stability, and durability. The Dasharo Entry Subscription guarantees continuous firmware development and fast firmware updates. 

[![Photo of NitroPC Pro](/attachment/posts/nitropc-pro.jpg)](https://shop.nitrokey.com/shop/product/nitropc-pro-523)

Here's a summary of the main component options available for this mid-tower desktop PC:

| Component                    | Options                                                  |
|----------------------------- | -------------------------------------------------------- |
| Motherboard                  | MSI PRO Z690-A DDR5 (Wi-Fi optional)                     |
| Processor                    | 12th Generation Intel Core i5-12600K or i9-12900K        |
| Memory                       | 16 GB to 128 GB DDR5                                     |
| NVMe storage (optional)      | Up to two NVMe PCIe 4.0 x4 SSDs, up to 2 TB each         |
| SATA storage (optional)      | Up to two SATA SSDs, up to 7.68 TB each                  |
| Wireless (optional)          | Wi-Fi 6E, 2400 Mbps, 802.11/a/b/g/n/ac/ax, Bluetooth 5.2 |
| Operating system (optional)  | Qubes OS 4.1 or Ubuntu 22.04 LTS                         |

**Important note:** When configuring your NitroPC Pro on the Nitrokey website, there is an option for a discrete graphics card (e.g., Nvidia GeForce RTX 4070 or 4090) in addition to integrated graphics (e.g., Intel UHD 770, which is always included because it is physically built into the CPU). NitroPC Pro configurations that include discrete graphics cards are *not* Qubes-certified. The only NitroPC Pro configurations that are Qubes-certified are those that contain *only* integrated graphics.

Of special note for Qubes users, the NitroPC Pro features a combined PS/2 port that supports both a PS/2 keyboard and a PS/2 mouse simultaneously with a Y-cable (not included). This allows for full control of dom0 without the need for USB keyboard or mouse passthrough. Nitrokey also offers a special tamper-evident shipping method for an additional fee. With this option, the case screws will be individually sealed and photographed, and the NitroPC Pro will be packed inside a sealed bag. Photographs of the seals will be sent to you by email, which you can use to determine whether the case was opened during transit.

The NitroPC Pro also comes with a "Dasharo Entry Subscription," which includes the following:

- Accesses to the latest firmware releases
- Exclusive newsletter
- Special firmware updates, including early access to updates enhancing privacy, security, performance, and compatibility
- Early access to new firmware releases for [newly-supported desktop platforms](https://docs.dasharo.com/variants/overview/#desktop) (please see the [roadmap](https://github.com/Dasharo/presentations/blob/main/dug2_dasharo_roadmap.md#dasharo-desktop-roadmap))
- Access to the Dasharo Premier Support invite-only live chat channel on the Matrix network, allowing direct access to the Dasharo Team and fellow subscribers with personalized and priority assistance
- Insider's view and influence on the Dasharo feature roadmap for a real impact on Dasharo development
- [Dasharo Tools Suite Entry Subscription](https://docs.dasharo.com/osf-trivia-list/dts/#what-is-dasharo-tools-suite-supporters-entrance) keys

For further product details, please see the official [NitroPC Pro](https://shop.nitrokey.com/shop/product/nitropc-pro-523) page.

## Special note regarding the need for `kernel-latest`

Beginning with Qubes OS 4.1.2, the Qubes installer includes the `kernel-latest` package and allows users to select this kernel option from the GRUB menu when booting the installer. At the time of this announcement, `kernel-latest` is **required** for the NitroPC Pro's graphics drivers to function properly. Therefore, all potential purchasers and users of this model should be aware that they will have to select a non-default option (`Install Qubes OS RX using kernel-latest`) from the GRUB menu when booting the installer. However, since Linux 6.1 has officially been promoted to being a long-term support (LTS) kernel, it will become the default kernel at some point, which means that the need for this non-default selection is only temporary.

## About Nitrokey

[Nitrokey](https://www.nitrokey.com/) is a world-leading company in open-source security hardware. Nitrokey develops IT security hardware for data encryption, key management and user authentication, as well as secure network devices, PCs, laptops, and smartphones. The company was founded in Berlin, Germany in 2015 and already counts tens of thousands of users from more than 120 countries, including numerous well-known international enterprises from various industries, among its customers. [Learn more.](https://www.nitrokey.com/about)

## About Dasharo

"Dasharo is an open-source firmware distribution focusing on seamless deployment, clean and simple code, long-term maintenance, professional support, transparent validation, superior documentation, privacy-respecting implementation, liberty for the owners and trustworthiness for all." ---the [Dasharo documentation](https://docs.dasharo.com/)

[Dasharo](https://www.dasharo.com/) is a registered trademark of and a product developed by [3mdeb](https://3mdeb.com/).

## What is Qubes-certified hardware?

[Qubes-certified hardware](/doc/certified-hardware/) is hardware that has been certified by the Qubes developers as compatible with a specific [major release](/doc/version-scheme/) of Qubes OS. All Qubes-certified devices are available for purchase with Qubes OS preinstalled. Beginning with Qubes 4.0, in order to achieve certification, the hardware must satisfy a rigorous set of [requirements](/doc/certified-hardware/#hardware-certification-requirements), and the vendor must commit to offering customers the very same configuration (same motherboard, same screen, same BIOS version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified computers](/doc/certified-hardware/#qubes-certified-computers) are specific models that are regularly tested by the Qubes developers to ensure compatibility with all of Qubes' features. The developers test all new major versions and updates to ensure that no regressions are introduced.

It is important to note, however, that Qubes hardware certification certifies only that a particular hardware *configuration* is *supported* by Qubes. The Qubes OS Project takes no responsibility for any vendor's manufacturing, shipping, payment, or other practices, nor can we control whether physical hardware is modified (whether maliciously or otherwise) *en route* to the user.
