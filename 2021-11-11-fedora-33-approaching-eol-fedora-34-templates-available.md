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

For a complete list of template releases supported for your specific
Qubes releases, see our [supported template releases].

Please note that no user action is required regarding the OS version in
dom0. For details, please see our [note on dom0 and EOL].


[end-of-life]: https://fedoraproject.org/wiki/End_of_life
[upgrade]: /doc/templates/fedora/#upgrading
[installation instructions]: /doc/templates/fedora/#installing
[performing an in-place upgrade]: /doc/template/fedora/upgrade/
[switching]: /doc/templates/#switching
[supported template releases]: /doc/supported-releases/#templates
[note on dom0 and EOL]: /doc/supported-releases/#note-on-dom0-and-eol
