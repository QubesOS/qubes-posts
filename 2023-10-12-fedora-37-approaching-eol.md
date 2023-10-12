---
layout: post
title: "Fedora 37 approaching EOL"
categories: announcements
---

Fedora 37 is currently [scheduled](https://fedorapeople.org/groups/schedule/f-39/f-39-key-tasks.html) to reach EOL ([end-of-life](https://fedoraproject.org/wiki/End_of_life)) on 2023-11-21. We strongly recommend that all users [upgrade](/doc/templates/fedora/#upgrading) their Fedora templates and standalones to [Fedora 38](/news/2023/05/26/fedora-38-templates-available/) before then. For more information, see [Upgrading to avoid EOL](/doc/how-to-update/#upgrading-to-avoid-eol).

There are two ways to upgrade your template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace the existing one.](/doc/templates/fedora/#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](/doc/templates/#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

For a complete list of template releases that are supported for your specific Qubes release, see our [supported template releases](/doc/supported-releases/#templates). Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](/doc/supported-releases/#note-on-dom0-and-eol)).
