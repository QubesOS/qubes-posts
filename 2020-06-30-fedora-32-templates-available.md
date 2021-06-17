---
layout: post
title: "Fedora 32 TemplateVMs available"
categories: announcements
author: Andrew David Wong
---

New Fedora 32 TemplateVMs are now available for both Qubes 4.0 and 4.1.

**Important:** If you wish to use the Qubes Update widget to update a
Fedora 32 template, you must first [switch][switching] the
`default-mgmt-dvm` qube to a Fedora 32 template. (Alternatively, you
can create a separate management DisposableVM Template based on a
Fedora 32 template for the purpose of updating Fedora 32 templates.)
This does not affect updating internally using `dnf`.

Instructions are available for [upgrading Fedora TemplateVMs]. We also
provide fresh Fedora 32 TemplateVM packages through the official Qubes
repositories, which you can get with the following commands (in dom0).

[Standard][Fedora] Fedora 32 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-32

[Minimal] Fedora 32 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-32-minimal

After installing or upgrading a TemplateVM, please remember to [update]
(see important note above) and [switch all qubes that were using the
old template to use the new one][switching].


[upgrading Fedora TemplateVMs]: /doc/template/fedora/upgrade/
[Fedora]: /doc/templates/fedora/
[Minimal]: /doc/templates/minimal/
[update]: /doc/how-to-install-software/
[switching]: /doc/templates/#switching

