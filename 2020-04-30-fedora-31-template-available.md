---
layout: post
title: "Fedora 30 approaching EOL, Fedora 31 TemplateVM available, Fedora 32 TemplateVM in testing"
categories: announcements
author: Andrew David Wong
---

This announcement includes several updates regarding Fedora TemplateVMs.

## Fedora 30 approaching EOL

With the release of Fedora 32 on April 28, Fedora 30 is expected to
reach EOL ([end-of-life]) on May 26, 2020.

## Fedora 31 TemplateVM available

A new Fedora 31 TemplateVM is now available for both Qubes 4.0 and 4.1.
Instructions are available for [upgrading Fedora TemplateVMs].  We also
provide a fresh Fedora 31 TemplateVM package through the official Qubes
repositories, which you can get with the following commands (in dom0).

[Standard] Fedora 31 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-31

[Minimal] Fedora 31 TemplateVM:

    $ sudo qubes-dom0-update qubes-template-fedora-31-minimal

After upgrading to a Fedora 31 TemplateVM, please remember to [switch all
qubes that were using the old template to use the new one][switching].

## Fedora 32 TemplateVM in testing

For advanced users, a new Fedora 32 TemplateVM is currently available in
the `qubes-templates-itl-testing` repository for both Qubes 4.0 and 4.1.
We would greatly appreciate [testing and feedback] from the community
regarding this template.


[end-of-life]: https://fedoraproject.org/wiki/End_of_life
[upgrading Fedora TemplateVMs]: /doc/template/fedora/upgrade/
[Standard]: /doc/templates/fedora/
[Minimal]: /doc/templates/minimal/
[switching]: /doc/templates/#switching
[testing and feedback]: /doc/testing/#providing-feedback
