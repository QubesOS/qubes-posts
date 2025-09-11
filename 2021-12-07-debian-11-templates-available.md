---
layout: post
title: "Debian 11 templates available"
categories: announcements
author: Andrew David Wong
---

New Debian 11 templates are available for both Qubes 4.0 and 4.1.

We provide fresh Debian 11 template packages through the official Qubes
repositories, which you can install in dom0 by following the standard
[installation instructions]. Alternatively, we also provide step-by-step
instructions for [performing an in-place upgrade] of an existing Debian
template. After upgrading your templates, please remember to [switch all
qubes that were using the old template to use the new one][switching].

For a complete list of template releases that are supported for your
specific Qubes release, see our [supported template releases].

Please note that no user action is required regarding the OS version in
dom0. For details, please see our [note on dom0 and EOL].

**Note for 4.1 release candidate testers:** Qubes 4.1 release
candidates already include the Debian 11 template by default, so no
action is required.


[installation instructions]: https://doc.qubes-os.org/en/latest/user/templates/debian/debian.html#installing
[performing an in-place upgrade]: /doc/templates/debian/in-place-upgrade/
[switching]: https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching
[supported template releases]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#templates
[note on dom0 and EOL]: https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol
