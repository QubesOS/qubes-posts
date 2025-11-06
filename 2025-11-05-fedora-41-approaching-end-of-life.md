---
layout: post
title: "Fedora 41 approaching end of life"
categories: announcements
---

Fedora 41 is currently [scheduled](https://fedorapeople.org/groups/schedule/f-41/f-41-key-tasks.html) to reach [end of life (EOL)](https://fedoraproject.org/wiki/End_of_life) on 2025-11-19 (approximately two weeks from the date of this announcement). Please upgrade all of your Fedora templates and standalones by that date. For more information, see [Upgrading to avoid EOL](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol).

There are two ways to upgrade a template to a new Fedora release:

- **Recommended:** [Install a new template to replace an existing one.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) This option is simpler for less experienced users, but it won't preserve any modifications you've made to your template. After you install the new template, you'll have to redo your desired template modifications (if any) and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). If you choose to modify your template, you may wish to write those modifications down so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the old Fedora template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora-upgrade.html) This option will preserve any modifications you've made to the template, but it may be more complicated for less experienced users.

Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).
