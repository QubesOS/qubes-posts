---
layout: post
title: "Fedora 39 approaching end-of-life"
categories: announcements
---

Fedora 39 is currently [scheduled](https://fedorapeople.org/groups/schedule/f-39/f-39-key-tasks.html) to reach [end-of-life (EOL)](https://fedoraproject.org/wiki/End_of_life) on 2024-11-12 (approximately two months from now). Please upgrade all of your Fedora templates and standalones by that date. For more information, see [Upgrading to avoid EOL](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol).

There are two ways to upgrade a template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace an existing one.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).
