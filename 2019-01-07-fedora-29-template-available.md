---
layout: post
title: "Fedora 29 TemplateVM available for Qubes 4.0"
date: 2019-01-07
categories: announcements
author: Andrew David Wong
---

A new Fedora 29 TemplateVM is now available for Qubes 4.0.  We
previously announced that [Fedora 27 reached EOL] and encouraged users
to upgrade to Fedora 28. Fedora 28 is still supported by the Fedora
Project, so users may now choose either Fedora 28 or 29 (or both)
depending on their needs and preferences.  Instructions are available
for [upgrading from Fedora 28 to 29].  We also provide fresh Fedora 29
TemplateVM packages through the official Qubes repositories, which you
can get with the following commands (in dom0).

Standard Fedora 29 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-29

[Minimal] Fedora 29 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-29-minimal

After upgrading to a Fedora 29 TemplateVM, please remember to set all
qubes that were using the old template to use the new one. This can be
done in dom0 either with the Qubes VM Manager or with the `qvm-prefs`
command-line tool.


[Fedora 27 reached EOL]: /news/2018/11/30/fedora-27-eol/
[upgrading from Fedora 28 to 29]: /doc/template/fedora/upgrade-28-to-29/
[Minimal]: /doc/templates/fedora-minimal/
