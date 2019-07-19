---
layout: post
title: "Insurgo PrivacyBeast X230 Laptop meets and exceeds Qubes 4.0 hardware certification"
categories: announcements
author: Andrew David Wong
---

We are very pleased to announce that the [Insurgo PrivacyBeast X230] has
passed Qubes 4.0 Hardware Certification and is now a [Qubes-certified
Laptop][laptop]!

## What is Qubes Certified Hardware?

[Qubes Certified Hardware] is hardware that has been certified by the
Qubes developers as compatible with Qubes OS. Beginning with Qubes 4.0,
in order to achieve certification, the hardware must satisfy a rigorous
set of [requirements], and the vendor must commit to offering customers
the very same configuration (same motherboard, same screen, same BIOS
version, same Wi-Fi module, etc.) for at least one year.

[Qubes-certified Laptops][laptop], in particular, are regularly tested
by the Qubes developers to ensure compatibility with all of Qubes'
features. The developers test all new major versions and updates to
ensure that no regressions are introduced.

It is important to note, however, that Qubes Hardware Certification
certifies only that a particular hardware *configuration* is *supported*
by Qubes. The Qubes OS Project takes no responsibility for any
manufacturing or shipping processes, nor can we control whether physical
hardware is modified (whether maliciously or otherwise) *en route* to
the user. (However, see below for information about how the Insurgo
team mitigates this risk.)

## About the Insurgo PrivacyBeast X230 Laptop

The [Insurgo PrivacyBeast X230] is a custom refurbished [ThinkPad X230]
that not only *meets* all Qubes Hardware Certification [requirements]
but also *exceeds* them thanks to its unique configuration, including:

  - [Coreboot] initialization for the x230 is binary-blob-free, including
    native graphic initialization. Built with the
    [Heads] payload, it delivers an [Anti Evil Maid (AEM)]-like solution
    built into the firmware. (Even though our [requirements] provide an
    exception for CPU-vendor-provided blobs for silicon and memory
    initialization, Insurgo exceeds our requirements by insisting that
    these be absent from its machines.)

  - [Intel ME] is neutered through the AltMeDisable bit, while all
    modules other than ROMP and BUP, which are required to initialize
    main CPU, have been [deleted][intel-me-deleted].

  - A re-ownership process that allows it to ship pre-installed with
    Qubes OS, including full-disk encryption already in place, but
    where the final disk encryption key is regenerated only when the
    machine is first powered on by the user, so that the OEM doesn't
    know it.

  - [Heads] provisioned pre-delivery to protect against malicious
    [interdiction].

## How to get one

Please see the [Insurgo PrivacyBeast X230] on the [Insurgo website]
for more information.

## Acknowledgements

Special thanks go to:

  - [Thierry Laurion], Director of Insurgo, Technologies Libres (Open
    Technologies), for spearheading this effort and making Heads+Qubes
    laptops more broadly accessible.

  - [Trammell Hudson], for creating [Heads].

  - [Purism], for greatly improving the UX of [Heads], including the GUI
    menu, and for adding [Nitrokey] and [Librem Key] support.


[Insurgo PrivacyBeast X230]: https://insurgo.ca/produit/qubesos-certified-privacybeast_x230-reasonably-secured-laptop/
[laptop]: https://www.qubes-os.org/doc/certified-hardware/#qubes-certified-laptops
[Qubes Certified Hardware]: https://www.qubes-os.org/doc/certified-hardware/
[requirements]: https://www.qubes-os.org/doc/certified-hardware/#hardware-certification-requirements
[ThinkPad X230]: https://www.thinkwiki.org/wiki/Category:X230
[Coreboot]: https://www.coreboot.org/
[Heads]: https://github.com/osresearch/heads/
[Anti Evil Maid (AEM)]: https://www.qubes-os.org/doc/anti-evil-maid/
[Intel ME]: https://libreboot.org/faq.html#intelme
[intel-me-deleted]: https://github.com/osresearch/heads-wiki/blob/master/Clean-the-ME-firmware.md#how-to-disabledeactive-most-of-it
[interdiction]: https://en.wikipedia.org/wiki/Interdiction
[Insurgo website]: https://insurgo.ca
[Thierry Laurion]: https://www.linkedin.com/in/thierry-laurion-40b4128/
[Trammell Hudson]: https://trmm.net/About
[Purism]: https://puri.sm/
[Nitrokey]: https://www.nitrokey.com/
[Librem Key]: https://puri.sm/posts/introducing-the-librem-key/

