---
layout: post
title: "Fedora 33 approaching EOL; Fedora 34 templates available"
categories: announcements
author: Andrew David Wong
---

Fedora 33 is scheduled to reach EOL ([end-of-life]) on 2021-11-30, and
new Fedora 34 templates are now available for both Qubes 4.0 and 4.1.

We strongly recommend that all Qubes users [upgrade] their Fedora 33
templates and standalones to Fedora 34 before Fedora 33 reaches EOL.

We provide fresh Fedora 34 template packages through the official Qubes
repositories, which you can install in dom0 by following the standard
[installation instructions]. Alternatively, we also provide step-by-step
instructions for [performing an in-place upgrade] of an existing Fedora
template. After upgrading your templates, please remember to [switch all
qubes that were using the old template to use the new one][switching].

For a complete list of template releases that are supported for your
specific Qubes release, see our [supported template releases].

Please note that no user action is required regarding the OS version in
dom0. For details, please see our [note on dom0 and EOL].

**Note for 4.1 release candidate testers:** Qubes R4.1-rc1 already
includes the Fedora 34 template by default, so no action is required.


[end-of-life]: https://fedoraproject.org/wiki/End_of_life
[upgrade]: https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#upgrading
[installation instructions]: https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing
[performing an in-place upgrade]: /doc/templates/fedora/in-place-upgrade/
[switching]: https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching
[supported template releases]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#templates
[note on dom0 and EOL]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol
