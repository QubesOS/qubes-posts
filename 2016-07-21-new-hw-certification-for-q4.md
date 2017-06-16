---
layout: post
title: Updated requirements for Qubes-certified hardware
author: Joanna Rutkowska
---

Last year, we [announced][original_announcement] the [Qubes-certified laptops
program]. The main purpose of the program is to provide a simple way for new
users to get a laptop that can run Qubes OS well. Apart from "running Qubes
well" we have not imposed any specific requirements thus far. Now, we're
introducing additional conditions, designed to also make such devices actually
more secure than what one can get in a supermarket. The rules below will become
obligatory for hardware to be certified for the upcoming Qubes 4.x branch. We
expect the first release candidate (4.0-rc1) to be out in September and the
final 4.0 sometime around the end of the year.

One of the most important security improvements that we [plan][pvh_ticket] to
introduce with the release of Qubes 4 is to ditch paravirtualization (PV)
technology and replace it with **hardware-enforced memory virtualization**,
which recent processors have made possible thanks to so-called Second Level
Address Translation ([SLAT]), also known as [EPT][EPT-enabled CPUs] in Intel
parlance.  SLAT (EPT) is an extension to Intel VT-x virtualization, which
originally was capable of only CPU virtualization but not memory virtualization
and hence required a complex Shadow Page Tables approach (which -- we believed
back then -- was actually less attractive than the PV approach). We hope that
embracing SLAT-based memory virtualization will allow us to prevent disastrous
security bugs, such as the infamous [XSA 148], publicly disclosed in October of
last year, which -- unlike many other major Xen bugs -- regrettably did
[affect][QSB 22] Qubes OS. Consequently, we will be requiring SLAT support of
all certified hardware for Qubes OS 4 and later.

Another important requirement we're introducing today is that Qubes-certified
hardware should run only **open-source boot firmware** (aka "the BIOS"), such as
[coreboot]. The only exception is the use of a (properly authenticated)
CPU-vendor-provided blobs for silicon and memory initialization (see [Intel
FSP]) as well as other internal operations (see [Intel ME]). However, we
specifically require all code used for and dealing with the System Management
Mode (SMM) to be open-source.

While [we well recognize][x86_harmful] the potential problems that proprietary
CPU-vendor code can cause, we are also pragmatic enough to realize that we need
to take smaller steps first, before we can implement even stronger
countermeasures such as the [stateless laptop] I proposed a few months ago.  A
switch to open source boot firmware is one such very important step on this
roadmap.

Of course, to be compatible with Qubes OS, the BIOS must properly expose
all the VT-x, VT-d, and SLAT functionality that the underlying hardware offers
(and which we require). Among other things, this implies **proper DMAR ACPI
table** construction.

Finally, we are going to require that Qubes-certified hardware does not have any
built-in _USB-connected_ microphones (e.g. as part of a USB-connected built-in
camera) that cannot be easily physically disabled by the user, e.g. via a
convenient mechanical switch. However, it should be noted that the majority of
laptops on the market that we have seen satisfy this condition out of the box,
because their built-in microphones are typically connected to the internal audio
device, which itself is a PCIe type of device. This is important, because such
PCIe audio devices are -- by default -- assigned to Qubes' (trusted) dom0 and
exposed through our carefully designed protocol only to select AppVMs when the
user explicitly chooses to do so. The rest of the time, they should be outside
the reach of malware.

While we also recommend a physical kill switch on the built-in camera (or, if
possible, not to have a built-in camera), we also recognize this isn't a
critical requirement, because users who are concerned about it can easily
deactivate it with... a piece of tape (something that, regrettably, cannot be
easily done with a microphone).

Similarly, we don't consider physical kill switches on Wi-Fi and Bluetooth
devices to be mandatory. Users who plan on using Qubes in an air-gap scenario
would do best if they manually remove all such devices persistently (as well as
the builtin [speakers][audio_modem]!), rather than rely on
easy-to-flip-by-mistake switches, while others should benefit from the Qubes
default sandboxing of all networking devices in dedicated VMs.

We hope these updated hardware requirements will encourage the development of
more secure and trustworthy devices capable of running Qubes OS and also pave
the way for even more ambitious solutions, such as the stateless laptop.

[original_announcement]: https://www.qubes-os.org/news/2015/12/09/purism-partnership/
[Qubes-certified laptops program]: https://www.qubes-os.org/doc/certified-laptops/
[SLAT]: https://en.wikipedia.org/wiki/Second_Level_Address_Translation
[EPT-enabled CPUs]: https://ark.intel.com/Search/FeatureFilter?productType=processors&ExtendedPageTables=true&MarketSegment=Mobile
[XSA 148]: https://xenbits.xen.org/xsa/advisory-148.html
[QSB 22]: https://github.com/QubesOS/qubes-secpack/blob/master/QSBs/qsb-022-2015.txt
[pvh_ticket]: https://github.com/QubesOS/qubes-issues/issues/2185
[coreboot]: https://www.coreboot.org/
[Intel FSP]: https://firmware.intel.com/learn/fsp/about-intel-fsp
[Intel ME]: https://www.apress.com/9781430265719
[x86_harmful]: https://blog.invisiblethings.org/papers/2015/x86_harmful.pdf
[stateless laptop]: https://blog.invisiblethings.org/papers/2015/state_harmful.pdf
[audio_modem]: https://github.com/romanz/amodem/
