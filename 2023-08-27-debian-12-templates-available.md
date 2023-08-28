---
layout: post
title: "Debian 12 templates available"
categories: announcements
---

The following new templates are now available:

**Qube OS 4.1**
- Debian 12
- Debian 12 [minimal](/doc/templates/minimal/)

**Qubes OS 4.2-rc1**
- Debian 12
- Debian 12 [minimal](/doc/templates/minimal/)
- Debian 12 [Xfce](/doc/templates/xfce/)

There are two ways to upgrade your template to a new Debian release:

- **Recommended:** [Install a fresh template to replace the existing one.](/doc/templates/debian/#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](/doc/templates/#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. In the old Debian template, see `/var/log/dpkg.log` and `/var/log/apt/history.log` for logs of package manager actions.

- **Advanced:** [Perform an in-place upgrade of an existing Debian template.](/doc/templates/debian/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

For a complete list of template releases that are supported for your specific Qubes release, see our [supported template releases](/doc/supported-releases/#templates). Please note that no user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](/doc/supported-releases/#note-on-dom0-and-eol)).
