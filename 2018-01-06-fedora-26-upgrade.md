---
layout: post
title: "Fedora 26 TemplateVM Upgrade"
date: 2018-01-06
categories: announcements
author: Andrew David Wong
---

Fedora 25 reached EOL ([end-of-life]) on 2017-12-12. We sincerely
apologize for our failure to provide timely notice of this event. It
is strongly recommend that all Qubes users upgrade their Fedora 25
TemplateVMs and StandaloneVMs to Fedora 26 immediately. We provide
step-by-step [upgrade instructions] for upgrading your existing
TemplateVMs and StandaloneVMs in-place on both Qubes 3.2 and Qubes
4.0. For a complete list of TemplateVM versions supported for your
specific version of Qubes, see [Supported TemplateVM Versions].

We also provide fresh Fedora 26 TemplateVM packages through the
official Qubes repositories, which you can get with the following
commands (in dom0).

Standard Fedora 26 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-26

[Minimal] Fedora 26 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-26-minimal

Afterward, you must update the template with the following command
(in the template):

    $ sudo dnf upgrade --best --allowerasing

After upgrading to a Fedora 26 TemplateVM, please remember to set all
qubes that were using the old template to use the new one. The
instructions to do this can be found in the [upgrade instructions]
for your specific version.

Please note that no user action is required regarding the OS version
in dom0. If you're using Qubes 3.2 or 4.0, there is no dom0 OS
upgrade available, since none is currently required. For details,
please see our [Note on dom0 and EOL].

If you're using an older version of Qubes than 3.2, we strongly
recommend that you upgrade to 3.2, as older versions are no longer
supported.


[end-of-life]: https://fedoraproject.org/wiki/Fedora_Release_Life_Cycle#Maintenance_Schedule
[upgrade instructions]: /doc/templates/fedora/#upgrading
[Supported TemplateVM Versions]: /doc/supported-versions/#templates
[Minimal]: /doc/templates/fedora-minimal/
[Note on dom0 and EOL]: /doc/supported-versions/#note-on-dom0-and-eol

