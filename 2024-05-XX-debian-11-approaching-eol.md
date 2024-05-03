---
layout: post
title: "Debian 11 (Bullseye) approaching EOL"
categories: announcements
---

The Debian Project currently [estimates](https://wiki.debian.org/DebianReleases) that Debian 11 (Bullseye) will reach EOL (end-of-life) sometime around July 2024 (approximately two months from now). Please upgrade all of your Debian 11 templates and standalones to [Debian 12 (Bookworm)](/news/2023/08/27/debian-12-templates-available/) by then. For general information about upgrading, see [Upgrading to avoid EOL](/doc/how-to-update/#upgrading-to-avoid-eol).

There are two ways to upgrade a template to a new Debian release:

- **Recommended:** [Install a fresh template to replace the existing one.](/doc/templates/debian/#installing) **This option may be simpler for less experienced users.** After you install the new template, redo all desired template modifications and [switch everything that was set to the old template to the new template](/doc/templates/#switching). You may want to write down the modifications you make to your templates so that you remember what to redo on each fresh install. In the old Debian template, see `/var/log/dpkg.log` and `/var/log/apt/history.log` for logs of package manager actions.

- **Advanced:** [Perform an in-place upgrade of an existing Debian template.](/doc/templates/debian/in-place-upgrade/) This option will preserve any modifications you've made to the template, **but it may be more complicated for less experienced users.**

## Note on LTS

Debian releases have two EOL dates: regular and [long-term support (LTS)](https://wiki.debian.org/LTS). See [Debian Production Releases](https://wiki.debian.org/DebianReleases#Production_Releases) for a chart that illustrates this. Qubes OS support for Debian templates ends at the regular EOL date, not the LTS EOL date.
