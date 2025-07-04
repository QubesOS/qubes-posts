---
layout: post
title: "Fedora 39 templates available; Fedora 38 approaching EOL"
categories: announcements
---

New Fedora 39 templates are now available in standard, [minimal](/doc/templates/minimal/), and [Xfce](/doc/templates/xfce/) varieties. In addition, Fedora 38 is currently [scheduled](https://fedorapeople.org/groups/schedule/f-38/f-38-key-tasks.html) to reach EOL ([end-of-life](https://fedoraproject.org/wiki/End_of_life)) on 2024-05-14 (approximately three months from now). Please upgrade all of your Fedora templates and standalones by that date. For more information, see [Upgrading to avoid EOL](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol).

There are two ways to upgrade a template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace an existing one.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).

## Special note on updating Fedora 39 templates on Qubes 4.1

In order to update Fedora 39 templates on Qubes 4.1, the default management disposable template (`default-mgmt-dvm`) must also be based on a Fedora 39 template. Here is the recommended order of events:

1. [Install](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) a fresh Fedora 39 template.
2. [Switch](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching) `default-mgmt-dvm` to the new Fedora 39 template.
3. [Update](/doc/how-to-update/) the Fedora 39 template.

By default, this applies only to Qubes 4.1, since the default update mechanism in Qubes 4.2 no longer relies on Salt. (However, if you have configured your Qubes 4.2 system so that it uses Salt for updates, then this still applies to you.)
