---
layout: post
title: "Recommended Fedora 25 TemplateVM Upgrade for Qubes 3.2"
date: 2017-07-29
categories: announcements
author: Andrew David Wong
---

Fedora 24, one of the supported [TemplateVM versions] in Qubes 3.2, is
scheduled to reach EOL (end of life) approximately
[one month][fedora-maintenance-schedule] after the release of Fedora 26,
which was on 2017-07-11. This places the expected EOL date for Fedora 24
somewhere in the second week of August.

Options for migrating to Fedora 25 are now available for Qubes 3.2.
If you're currently using Fedora 24 (or older) TemplateVMs or StandaloneVMs,
we strongly recommend upgrading them to Fedora 25 before then. We provide
[step-by-step instructions][upgrade] for upgrading your existing TemplateVMs
and StandaloneVMs in-place.

We also provide fresh Fedora 25 TemplateVM packages through the official
Qubes repositories, which you can get with the following commands (in dom0).

Standard Fedora 25 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-25

[Minimal] Fedora 25 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-25-minimal

After upgrading to a Fedora 25 TemplateVM, please remember to set all
qubes that were using the old template to use the new one. This can be
done in dom0 either with the Qubes VM Manager or with the `qvm-prefs`
command-line tool.

Please note that no user action is required regarding the OS version in
dom0. If you're using Qubes 3.2, there is no dom0 OS upgrade available,
since none is currently required. For details, please see [here][dom0].

If you're using an older version of Qubes, we strongly recommend that
you upgrade to 3.2, as older versions are no longer supported.


[TemplateVM versions]: /doc/supported-versions/#templates
[fedora-maintenance-schedule]: https://fedoraproject.org/wiki/Fedora_Release_Life_Cycle#Maintenance_Schedule
[upgrade]: /doc/templates/fedora/#upgrading
[Minimal]: /doc/templates/fedora-minimal/
[dom0]: /doc/supported-versions/#dom0

