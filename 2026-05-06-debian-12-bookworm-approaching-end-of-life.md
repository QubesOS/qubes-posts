---
layout: post
title: "Debian 12 (Bookworm) approaching end of life"
categories: announcements
---

Debian 12 (Bookworm) is currently [scheduled](https://www.debian.org/releases/) to reach end-of-life (EOL) on 2026-06-10 (approximately one month from the date of this announcement). Please upgrade all of your Debian templates and standalones by that date. For more information, see [Upgrading to avoid EOL](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol).

There are two ways to upgrade a template to a new Debian release:

- **Recommended:** [Install a fresh template to replace the existing one.](https://doc.qubes-os.org/en/latest/user/templates/debian/debian.html#installing) This option is simpler for less experienced users, but it won't preserve any modifications you've made to your template. After you install the new template, you'll have to redo your desired template modifications (if any) and [switch everything that was set to the old template to the new template](https://doc.qubes-os.org/en/latest/user/templates/templates.html#switching). If you choose to modify your template, you may wish to write those modifications down so that you remember what to redo on each fresh install. To see a log of package manager actions, see `/var/log/dpkg.log` and `/var/log/apt/history.log` in the old Debian template.

- **Advanced:** [Perform an in-place upgrade of an existing Debian template.](https://doc.qubes-os.org/en/latest/user/templates/debian/debian-upgrade.html) This option will preserve any modifications you've made to the template, but it may be more complicated for less experienced users.

## Note on Debian LTS

Debian releases have two EOL dates: regular and [long-term support (LTS)](https://www.debian.org/lts/). Qubes OS support for Debian templates ends at the regular EOL date, not the LTS EOL date.
