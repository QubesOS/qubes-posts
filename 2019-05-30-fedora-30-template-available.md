---
layout: post
title: "Fedora 30 TemplateVM available"
categories: announcements
author: Andrew David Wong
---

A new Fedora 30 TemplateVM is now available.  We
previously announced that [Fedora 28 reached EOL] and encouraged users
to upgrade to Fedora 29. Fedora 29 is still supported by the Fedora
Project, so users may now choose either Fedora 29 or 30 (or both)
depending on their needs and preferences.  Instructions are available
for [upgrading from Fedora 29 to 30].  We also provide fresh Fedora 30
TemplateVM packages through the official Qubes repositories, which you
can get with the following commands (in dom0).

Standard Fedora 30 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-30

[Minimal] Fedora 30 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-30-minimal

After upgrading to a Fedora 30 TemplateVM, please remember to set all
qubes that were using the old template to use the new one. This can be
done in dom0 either with the [Qubes Template Manager] or with the
[`qvm-prefs`] command-line tool.


[Fedora 28 reached EOL]: /news/2019/05/29/fedora-28-eol/
[upgrading from Fedora 29 to 30]: /doc/template/fedora/upgrade-29-to-30/
[Minimal]: /doc/templates/fedora-minimal/
[Qubes Template Manager]: /doc/templates/
[`qvm-prefs`]: https://dev.qubes-os.org/projects/core-admin-client/en/latest/manpages/qvm-prefs.html
