---
layout: post
title: "Fedora 40 templates available"
categories: announcements
---

New Fedora 40 templates are now available for Qubes OS 4.2 in standard, [minimal](/doc/templates/minimal/), and [Xfce](/doc/templates/xfce/) varieties. There are two ways to upgrade a template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace an existing one.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

Please note:
- This announcement concerns only Qubes 4.2. Fedora 40 templates will not be available for Qubes 4.1.
- No user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).
