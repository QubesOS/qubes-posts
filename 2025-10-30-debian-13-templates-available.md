---
layout: post
title: "Debian 13 templates available"
categories: announcements
---

New Debian 13 templates are now available for both Qubes OS 4.2 (stable) and Qubes OS 4.3 (release candidates) in [standard](https://doc.qubes-os.org/en/latest/user/templates/debian/debian.html), [minimal](https://doc.qubes-os.org/en/latest/user/templates/minimal-templates.html), and [Xfce](https://doc.qubes-os.org/en/latest/user/templates/xfce-templates.html) varieties. There are two ways to upgrade a template to a new Debian release:

- **Recommended:** [Install a fresh template to replace the existing one.](https://doc.qubes-os.org/en/latest/user/templates/debian/debian.html#installing) This option is simpler for less experienced users, but it won't preserve any modifications you've made to your template. After you install the new template, you'll have to redo your desired template modifications (if any) and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). If you choose to modify your template, you may wish to write those modifications down so that you remember what to redo on each fresh install. In the old Debian template, see `/var/log/dpkg.log` and `/var/log/apt/history.log` for logs of package manager actions.

- **Advanced:** [Perform an in-place upgrade of an existing Debian template.](https://doc.qubes-os.org/en/latest/user/templates/debian/debian-upgrade.html) This option will preserve any modifications you've made to the template, but it may be more complicated for less experienced users.

**Note:** No user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#note-on-dom0-and-eol)).
