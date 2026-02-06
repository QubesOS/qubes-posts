---
layout: post
title: "Fedora 43 templates available"
categories: announcements
---

The following new [Fedora 43 templates](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html) are now available for Qubes OS 4.3:

- `fedora-43-xfce` (default Fedora template with the [Xfce](https://xfce.org/) desktop environment)
- `fedora-43` (alternative Fedora template with the [GNOME](https://www.gnome.org/) desktop environment)
- `fedora-43-minimal` ([minimal template](https://doc.qubes-os.org/en/latest/user/templates/minimal-templates.html) for advanced users)

There are two ways to upgrade a template to a new Fedora release:

1. **Recommended:** [Install a fresh template to replace an existing one.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora.html#installing) This option is simpler for less experienced users, but it won't preserve any modifications you've made to your template. After you install the new template, you'll have to redo your desired template modifications (if any) and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). If you choose to modify your template, you may wish to write those modifications down so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the template and use the `dnf history` command.

2. **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](https://doc.qubes-os.org/en/latest/user/templates/fedora/fedora-upgrade.html) This option will preserve any modifications you've made to the template, but it may be more complicated for less experienced users.

**Note:** No user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).
