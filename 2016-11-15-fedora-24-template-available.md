---
layout: post
title: "Fedora 24 template now available for Qubes 3.2"
date: 2016-11-15
categories: announcements
author: Andrew David Wong
---

Fedora 23, the default Fedora [TemplateVM version] in Qubes 3.2, is scheduled to
reach end of life (EOL) on approximately 2016-12-22
([one month][fedora-maintenance-schedule] after the release of Fedora 25). **We
strongly advise all Qubes users to upgrade their Fedora templates before then.**

In the interest of providing users with a convenient upgrade path, a Fedora 24
template is now available for direct installation. This means that there are now
two ways to have a Fedora 24 template on Qubes 3.2:

1. [Upgrade] an existing Fedora 23 template.

2. Install a new Fedora 24 template by running the following command in dom0:

        $ sudo qubes-dom0-update qubes-template-fedora-24

The later option will result in a fresh template, which you may customize if
you wish. A Fedora 24 [minimal] template is also available, and it can be
installed by running the following command in dom0:

    $ sudo qubes-dom0-update qubes-template-fedora-24-minimal

After upgrading to a Fedora 24 template, please remember to switch the
appropriate qubes (VMs) to the new template. This can be done either with Qubes
Manager (in qube settings) or with the `qvm-prefs` command-line tool.


[TemplateVM version]: /doc/supported-versions/#templates
[fedora-maintenance-schedule]: https://fedoraproject.org/wiki/Fedora_Release_Life_Cycle#Maintenance_Schedule
[Upgrade]: /doc/templates/fedora/#upgrading
[minimal]: /doc/templates/fedora-minimal/
