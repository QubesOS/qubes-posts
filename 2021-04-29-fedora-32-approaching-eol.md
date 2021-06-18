---
layout: post
title: "Fedora 32 approaching EOL"
categories: announcements
author: Andrew David Wong
---

Fedora 32 is scheduled to reach EOL ([end-of-life]) on 2021-05-25.

We strongly recommend that all Qubes users [upgrade] their Fedora 32
TemplateVMs and StandaloneVMs to Fedora 33 before Fedora 32 reaches
EOL.

We provide a fresh Fedora 33 TemplateVM package through the official
Qubes repositories, which you can install in dom0 by following the
standard [installation instructions]. Alternatively, we also provide
step-by-step instructions for [performing an in-place upgrade] of an
existing Fedora TemplateVM. After upgrading your TemplateVMs, please
remember to [switch all qubes that were using the old template to use
the new one][switching].

For a complete list of TemplateVM versions supported for your specific
version of Qubes, see [Supported TemplateVM Versions].

Please note that no user action is required regarding the OS version in
dom0. For details, please see our [note on dom0 and EOL].


[end-of-life]: https://fedoraproject.org/wiki/End_of_life
[upgrade]: /doc/templates/fedora/#upgrading
[installation instructions]: /doc/templates/fedora/#installing
[performing an in-place upgrade]: /doc/template/fedora/upgrade/
[Supported TemplateVM Versions]: /doc/supported-versions/#templates
[switching]: /doc/templates/#switching
[note on dom0 and EOL]: /doc/supported-versions/#note-on-dom0-and-eol

