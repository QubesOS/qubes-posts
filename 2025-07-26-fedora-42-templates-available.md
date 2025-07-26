---
layout: post
title: "Fedora 42 templates available"
categories: announcements
---

New Fedora 42 templates are now available for Qubes OS 4.2 in standard, [minimal](/doc/templates/minimal/), and [Xfce](/doc/templates/xfce/) varieties. There are two ways to upgrade a template to a new Fedora release:

- **Recommended:** [Install a fresh template to replace an existing one.](/doc/templates/fedora/#installing) This option is simpler for less experienced users, but it won't preserve any modifications you've made to your template. After you install the new template, you'll have to redo all of your desired template modifications (if any) and [switch everything that was set to the old template to the new template](/doc/templates/#switching). If you modify your template, you may want to write those modifications down so that you remember what to redo on each fresh install. To see a log of package manager actions, open a terminal in the template and use the `dnf history` command.

- **Advanced:** [Perform an in-place upgrade of an existing Fedora template.](/doc/templates/fedora/in-place-upgrade/) This option will preserve any modifications you've made to the template, but it may be more complicated for less experienced users.

**Note:** No user action is required regarding the OS version in dom0 (see our [note on dom0 and EOL](/doc/supported-releases/#note-on-dom0-and-eol)).
