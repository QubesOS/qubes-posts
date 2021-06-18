---
layout: post
title: "Fedora 31 approaching EOL"
categories: announcements
author: Andrew David Wong
---

Fedora 33 was [released today], 2020-10-27. According to the [Fedora
Release Life Cycle], this means that Fedora 31 is scheduled to reach
EOL ([end-of-life]) in approximately four weeks, around [2020-11-24].

We strongly recommend that all Qubes users upgrade their Fedora 31
TemplateVMs and StandaloneVMs to Fedora 32 or higher before Fedora 31
reaches EOL. We provide step-by-step upgrade instructions for [upgrading
Fedora TemplateVMs]. For a complete list of TemplateVM versions
supported for your specific version of Qubes, see [Supported TemplateVM
Versions].

We also provide a fresh Fedora 32 TemplateVM package through the
official Qubes repositories, which you can install in dom0 by following
the standard [installation instructions].

After upgrading your TemplateVMs, please remember to [switch all qubes
that were using the old template to use the new one][switching].

Please note that no user action is required regarding the OS version in
dom0. For details, please see our [note on dom0 and EOL].


[released today]: https://fedoramagazine.org/announcing-fedora-33/
[Fedora Release Life Cycle]: https://fedoraproject.org/wiki/Fedora_Release_Life_Cycle
[2020-11-24]: https://www.timeanddate.com/date/dateadded.html?m1=10&d1=27&y1=2020&type=add&ay=&am=&aw=4&ad=&rec=
[end-of-life]: https://fedoraproject.org/wiki/End_of_life
[upgrading Fedora TemplateVMs]: /doc/template/fedora/upgrade/
[Supported TemplateVM Versions]: /doc/supported-versions/#templates
[installation instructions]: /doc/templates/fedora/#installing
[switching]: /doc/templates/#switching
[note on dom0 and EOL]: /doc/supported-versions/#note-on-dom0-and-eol

