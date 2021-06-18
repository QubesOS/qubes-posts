---
layout: post
title: "Fedora 26 and Debian 8 approaching EOL"
date: 2018-05-23
categories: announcements
author: Andrew David Wong
---

Fedora 26 will reach EOL ([end-of-life]) on 2018-05-29, and Debian 8
(["Jessie" full, not LTS][debian-releases]) will reach EOL on
2018-06-06. We strongly recommend that all Qubes users upgrade their
Fedora 26 TemplateVMs and StandaloneVMs to Fedora 27 or higher by
2018-06-01. Debian 8 users may choose to rely on Debian 8 LTS updates
until approximately 2020-06-06 or upgrade to Debian 9 at their
discretion. We provide step-by-step upgrade instructions for upgrading
from [Fedora 26 to 27], [Fedora 27 to 28], and [Debian 8 to 9]. For a
complete list of TemplateVM versions supported for your specific version
of Qubes, see [Supported TemplateVM Versions].

We also provide fresh Fedora 27, Fedora 28, and Debian 9 TemplateVM
packages through the official Qubes repositories, which you can install
in dom0 by following the standard installation instructions for [Fedora]
and [Debian] TemplateVMs.

After upgrading your TemplateVMs, please remember to set all qubes that
were using the old template to use the new one. The instructions to do
this can be found in the upgrade instructions for [Fedora 26 to 27],
[Fedora 27 to 28], and [Debian 8 to 9].

Please note that no user action is required regarding the OS version in
dom0. If you're using Qubes 3.2 or 4.0, there is no dom0 OS upgrade
available, since none is currently required. For details, please see our
[Note on dom0 and EOL].

If you're using an older version of Qubes than 3.2, we strongly
recommend that you upgrade to 3.2, as older versions are no longer
supported.


[end-of-life]: https://fedoraproject.org/wiki/Fedora_Release_Life_Cycle#Maintenance_Schedule
[debian-releases]: https://wiki.debian.org/DebianReleases
[Fedora 26 to 27]: /doc/template/fedora/upgrade-26-to-27/
[Fedora 27 to 28]: /doc/template/fedora/upgrade-27-to-28/
[Debian 8 to 9]: /doc/template/debian/upgrade-8-to-9/
[Supported TemplateVM Versions]: /doc/supported-versions/#templates
[Fedora]: /doc/templates/fedora/#installing
[Debian]: /doc/templates/debian/#installing
[Note on dom0 and EOL]: /doc/supported-versions/#note-on-dom0-and-eol

