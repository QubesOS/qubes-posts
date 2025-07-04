---
layout: post
title: "Minimum requirements for Qubes OS 4.x and extended support for Qubes OS 3.2"
date: 2016-09-02
categories:
  - announcements
  - releases
author: Andrew David Wong
---

We previously announced the [new hardware certification requirements for Qubes
4.x][hw-cert-q4]. Today, we're announcing the minimum requirements for this
release series. In light of these new minimum requirements, we're also
announcing extended support for Qubes OS 3.2.

(For background information, please see the Qubes [Version Scheme] and
[supported releases].)

Types of system requirements
----------------------------
There are three types of system requirements specified for Qubes OS:

 * Hardware certification requirements
 * Recommended system requirements
 * Minimum system requirements

[Hardware certification] requirements are the most stringent. With these
requirements, we aim to set the highest reasonable standard of security and
privacy available.

Recommended system requirements are typically less specific than hardware
certification requirements. These highlight what we consider to be the most
important to the Qubes user experience.

Minimum system requirements are the least stringent. These requirements specify
the bare necessities that a device must meet in order to run Qubes at all.

You can always view both the minimum and recommended system requirements for all
Qubes OS releases [here][sys-req].

Minimum requirements for Qubes OS 4.x
-------------------------------------
Here are the minimum requirements for Qubes OS 4.x:

 * 64-bit Intel or AMD processor (x86\_64 aka x64 aka AMD64)
 * Intel VT-x with EPT or AMD-V with RVI
 * Intel VT-d or AMD-Vi
 * 4 GB RAM
 * 32 GB disk space

In past Qubes OS releases, [Intel VT-x]/[AMD-V] and [Intel VT-d]/[AMD-Vi (aka
AMD IOMMU)] were recommended but not required. In the 4.x release series, both
will be required. Moreover, Intel VT-x must support [Extended Page Tables
(EPT)][EPT], and AMD-V must support [Rapid Virtualization Indexing (RVI)][RVI].
EPT and RVI are Intel's and AMD's respective implementations of [Second Level
Address Translation (SLAT)][SLAT]. For the rationale behind this requirement,
please see the [new hardware certification requirements for Qubes 4.x
announcement][hw-cert-q4].

In the interest of making it easier for users to find out whether their
processors will be supported by Qubes 4.x and to make purchasing decisions, here
is a [list of Intel processors that meet the minimum requirements for
Qubes 4.x][Intel-list]. (This list should automatically be updated whenever
Intel releases new processors.) Unfortunately, there is no comparable list of
AMD processors. (A list of IOMMU-supporting hardware, including AMD processors,
is available [here][IOMMU-list], but note that IOMMU is only one of the
requirements.) Users are advised to check the specifications of any AMD
processors they wish to use with Qubes 4.x.

In addition, we recommend consulting the [Hardware Compatibility List
(HCL)][HCL] for real-world compatibility test results. The HCL is useful thanks
to users who have contributed their results to it, so we kindly ask that you
consider [submitting an HCL report] of your own.

Extended Support for Qubes OS 3.2
---------------------------------
We realize that the new minimum requirements for Qubes OS 4.x may be difficult
for some users to meet. In light of this, we will extend the support period for
Qubes 3.2 (which is currently in the final stages of testing) to **one full
year** after the release of Qubes 4.0 (instead of [the usual six
months][supported releases]). 

In addition, the `qubes-hcl-report` tool, which is used for  [submitting an HCL
report], will be updated in Qubes 3.2. It will report on all the processor
features listed in the minimum requirements for Qubes 4.x, which will make it
easier for users of Qubes 3.2 to evaluate whether their machines will be
supported by the 4.x series. We hope that this, along with the flexibility
provided by the extended 3.2 support period, will help to ease the transition to
the 4.x series.


[hw-cert-q4]: /news/2016/07/21/new-hw-certification-for-q4/
[Version Scheme]: /doc/version-scheme/
[supported releases]: /doc/supported-releases/
[Hardware certification]: /hardware-certification/
[sys-req]:https://www.qubes-os.org/doc/system-requirements/
[Intel VT-x]: https://en.wikipedia.org/wiki/X86_virtualization#Intel_virtualization_.28VT-x.29
[AMD-V]: https://en.wikipedia.org/wiki/X86_virtualization#AMD_virtualization_.28AMD-V.29
[Intel VT-d]: https://en.wikipedia.org/wiki/X86_virtualization#Intel-VT-d
[AMD-Vi (aka AMD IOMMU)]: https://en.wikipedia.org/wiki/X86_virtualization#I.2FO_MMU_virtualization_.28AMD-Vi_and_Intel_VT-d.29
[EPT]: https://en.wikipedia.org/wiki/Second_Level_Address_Translation#Extended_Page_Tables
[RVI]: https://en.wikipedia.org/wiki/Second_Level_Address_Translation#Rapid_Virtualization_Indexing
[SLAT]: https://en.wikipedia.org/wiki/Second_Level_Address_Translation
[Intel-list]: https://ark.intel.com/Search/FeatureFilter?productType=processors&InstructionSet=64-bit&ExtendedPageTables=true&VTD=true&EM64=true
[IOMMU-list]: https://en.wikipedia.org/wiki/List_of_IOMMU-supporting_hardware
[HCL]: /hcl/
[submitting an HCL report]: https://doc.qubes-os.org/en/latest/user/hardware/how-to-use-the-hcl.html#generating-and-submitting-new-reports
