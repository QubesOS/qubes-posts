---
layout: post
title: "NitroPad T430 passes hardware certification for Qubes 4.0!"
categories: announcements
author: Andrew David Wong
image: /attachment/site/nitropad-t430.jpg
---

_**Update:** Please be advised that the i7-3632QM option is **not** compatible with Qubes OS, as it does not support VT-d. The option specifically tested by the Qubes team is the i5-3320M._

It is our pleasure to announce that the [NitroPad T430] has become the
third [Qubes-certified Laptop][laptop] for Qubes 4.0! This makes
[Nitrokey] the first vendor to have *two* products that pass Qubes
hardware certification, the other being the [NitroPad X230].


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
by Qubes. The Qubes OS Project takes no responsibility for any vendor's
manufacturing, shipping, payment, or other practices, nor can we control
whether physical hardware is modified (whether maliciously or otherwise)
*en route* to the user. (However, see below for information about how
this risk is mitigated.)


## About the NitroPad T430

[![nitropad-t430.jpg](/attachment/site/nitropad-t430.jpg)][NitroPad T430]

Key features of the [NitroPad T430] include:

  - Tamper detection through measured boot with [Coreboot], [Heads], and
    Nitrokey USB hardware, including support for [Anti Evil Maid (AEM)]

  - Deactivated [Intel Management Engine]

  - User-replaceable cryptographic keys

  - Included Nitrokey USB key

  - Professional ThinkPad hardware based on the [ThinkPad T430]

  - Security-conscious shipping to mitigate against third-party
    [interdiction]

For further details, please see the [NitroPad T430] product page.


## How to get one

Please see the [NitroPad T430] on the [Nitrokey website][Nitrokey] for
purchasing information.


[NitroPad T430]: https://shop.nitrokey.com/shop/product/nitropad-t430-119
[Nitrokey]: https://www.nitrokey.com/
[laptop]: /doc/certified-hardware/#qubes-certified-laptops
[NitroPad X230]: /doc/certified-hardware/#nitropad-x230
[Qubes Certified Hardware]: /doc/certified-hardware/
[requirements]: /doc/certified-hardware/#hardware-certification-requirements
[ThinkPad T430]: https://www.thinkwiki.org/wiki/Category:T430
[Coreboot]: https://www.coreboot.org/
[Heads]: https://github.com/osresearch/heads/
[Anti Evil Maid (AEM)]: /doc/anti-evil-maid/
[Intel Management Engine]: https://libreboot.org/faq.html#intelme
[interdiction]: https://en.wikipedia.org/wiki/Interdiction

