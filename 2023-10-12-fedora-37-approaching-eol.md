---
layout: post
title: "Fedora 37 approaching EOL"
categories: announcements
---

Fedora 37 is currently [scheduled](https://fedorapeople.org/groups/schedule/f-39/f-39-key-tasks.html) to reach EOL ([end-of-life](https://fedoraproject.org/wiki/End_of_life)) on 2023-11-21. We strongly recommend that all users [upgrade](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#upgrading) their Fedora templates and standalones to [Fedora 38](/news/2023/05/26/fedora-38-templates-available/) before then. For more information, see [Upgrading to avoid EOL](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol).

There are two ways to upgrade your template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace the existing one.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

For a complete list of template releases that are supported for your specific Qubes release, see our [supported template releases](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#templates). Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).
