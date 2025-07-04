---
layout: post
title: "Fedora 36 templates available"
categories: announcements
author: Andrew David Wong
---

New Fedora 36 templates are now available for Qubes 4.1!

Please note that Fedora 36 will not be available for Qubes 4.0, since
Qubes 4.0 is [scheduled to reach end-of-life (EOL) soon]. Fedora 35 is
[already available] for both Qubes 4.0 and 4.1 and will remain supported
until long after Qubes 4.0 has reached EOL.

As a reminder, [Fedora 34 has reached EOL]. If you have not already done
so, we strongly recommend that you [upgrade] any remaining Fedora 34
templates and standalones to Fedora 35 or 36 immediately.

We provide fresh Fedora 36 template packages through the official Qubes
repositories, which you can install in dom0 by following the standard
[installation instructions]. Alternatively, we also provide step-by-step
instructions for [performing an in-place upgrade] of an existing Fedora
template. After upgrading your templates, please remember to [switch all
qubes that were using the old template to use the new one][switching].

For a complete list of template releases that are supported for your
specific Qubes release, see our [supported template releases].

Please note that no user action is required regarding the OS version in
dom0. For details, please see our [note on dom0 and EOL].

**Note for release candidate testers:** [Qubes OS R4.1.1-rc1] already
includes a Fedora 36 template by default, so no action is required on
your part.


[scheduled to reach end-of-life (EOL) soon]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#qubes-os
[already available]: /news/2022/05/26/fedora-34-approaching-eol-fedora-35-templates-available/
[Fedora 34 has reached EOL]: /news/2022/06/07/fedora-34-eol/
[upgrade]: https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#upgrading
[installation instructions]: https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing
[performing an in-place upgrade]: /doc/templates/fedora/in-place-upgrade/
[switching]: https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching
[supported template releases]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#templates
[note on dom0 and EOL]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol
[Qubes OS R4.1.1-rc1]: /news/2022/06/27/qubes-4-1-1-rc1/
