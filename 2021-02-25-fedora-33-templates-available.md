---
layout: post
title: "Fedora 33 TemplateVMs available"
categories: announcements
author: Andrew David Wong
---

New Fedora 33 TemplateVMs are now available for both Qubes 4.0 and 4.1.

**Important:** If you wish to use the Qubes Update widget to update a Fedora 33 template, you must first [switch][switching] the `default-mgmt-dvm` qube to a Fedora 33 template. (Alternatively, you can create a separate management DisposableVM Template based on a Fedora 33 template for the purpose of updating Fedora 33 templates.) This does not affect updating internally using `dnf`.

**Note:** Fedora 33 has switched the default DNS resolver to [systemd-resolved]. If resolving local domains on your LAN does not work as expected even when specifying the full name, you may wish to disable systemd-resolved and enable NetworkManager in the TemplateVM instead.

Instructions are available for [upgrading Fedora TemplateVMs]. We also provide fresh Fedora 33 TemplateVM packages through the official Qubes repositories, which you can get with the following commands (in dom0).

[Standard][Fedora] Fedora 33 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-33

[Minimal] Fedora 33 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-33-minimal

After installing or upgrading a TemplateVM, please remember to [update] (see important note above) and [switch all qubes that were using the old template to use the new one][switching].

For more information about Fedora 33, please see the [Fedora 33 Release Notes] and the [Fedora 33 Announcement].


[upgrading Fedora TemplateVMs]: /doc/template/fedora/upgrade/
[Fedora]: /doc/templates/fedora/
[Minimal]: /doc/templates/minimal/
[update]: /doc/how-to-install-software/
[switching]: /doc/templates/#switching
[systemd-resolved]: https://fedoraproject.org/wiki/Changes/systemd-resolved
[Fedora 33 Release Notes]: https://docs.fedoraproject.org/en-US/fedora/f33/release-notes/
[Fedora 33 Announcement]: https://fedoramagazine.org/announcing-fedora-33/
