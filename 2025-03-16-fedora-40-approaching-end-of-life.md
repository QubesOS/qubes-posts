---
layout: post
title: "Fedora 40 approaching end of life"
categories: announcements
---

Fedora 40 is currently [scheduled](https://fedorapeople.org/groups/schedule/f-40/f-40-key-tasks.html) to reach [end of life (EOL)](https://fedoraproject.org/wiki/End_of_life) on 2025-05-13 (approximately two months from the date of this announcement). Please upgrade all of your Fedora templates and standalones by that date. For more information, see [Upgrading to avoid EOL](/doc/how-to-update/#upgrading-to-avoid-eol).

There are two ways to upgrade a template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace an existing one.](/doc/templates/fedora/#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](/doc/templates/#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](/doc/supported-releases/#note-on-dom0-and-eol)).
