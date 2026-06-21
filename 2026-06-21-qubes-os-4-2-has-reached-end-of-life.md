---
layout: post
title: "Qubes OS 4.2 has reached end of life"
categories:
 - announcements
 - releases
---

As [previously announced](https://www.qubes-os.org/news/2026/04/27/qubes-os-4-2-approaching-end-of-life/), the Qubes OS 4.2 release series has officially reached end of life (EOL) as of today, 2026-06-21. We strongly urge any remaining Qubes 4.2 users to [upgrade to Qubes 4.3](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html) immediately.

## Recommended actions

If you're already using Qubes 4.3, then you don't have to do anything. This announcement doesn't apply to you.

If you're still using Qubes 4.2, then you should upgrade to Qubes 4.3 as soon as possible. There are two ways to do this:

- [Perform a clean installation of Qubes 4.3](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html#clean-installation-of-qubes-4-3) using the latest stable [Qubes OS 4.3.1 ISO](https://www.qubes-os.org/news/2026/06/11/qubes-os-4-3-1-has-been-released/). This involves [backing up your current system](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html), replacing your current installation with a [fresh Qubes 4.3 installation](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/installation-guide.html), then [restoring from your backup](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html#restoring-from-a-backup). Many users find this option to be simpler, easier, and less error-prone. However, if you've made extensive customizations in dom0, they may need to be redone.

- [Perform an in-place upgrade from Qubes 4.2 to Qubes 4.3.](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html#in-place-upgrade-from-qubes-4-2-to-qubes-4-3) Instead of replacing your existing installation, this method involves installing a special command-line tool in dom0, then using it to upgrade your existing Qubes 4.2 installation to Qubes 4.3. This is a more complex multi-stage process, which makes it most suitable for advanced users. This method preserves your qubes and all the customizations you've made in dom0 that are compatible with the in-place upgrade process. While not strictly required, we still strongly recommend [making a full backup](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html) before attempting an in-place upgrade. This way, if anything goes wrong, your data is still safe, and you always have the option of falling back to performing a clean installation.

If you need help, please consult our [help and support](https://doc.qubes-os.org/en/latest/introduction/support.html) page.

## What does end of life (EOL) mean?

When an operating system reaches end of life (EOL), it is no longer supported. This means that it will no longer receive security updates, bug fixes, or new features. An OS that does not receive security updates will not be protected against new vulnerabilities, which is why it's critically important to upgrade to a supported release.

## What about patch releases?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. When a major or minor release reaches EOL, all of its patch releases also reach EOL. In this case, when we say that "Qubes 4.2" (without specifying a `[patch]` number) has reached EOL, we're specifying a particular minor release inclusive of all patch releases within it. This means that Qubes 4.2.0, 4.2.1, 4.2.2, 4.2.3, and 4.2.4 have all reached EOL, since they're all patch releases of the same minor release.

## How are EOL dates determined?

According to our [release support policy](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html), stable Qubes OS releases are supported for six months after each subsequent [major or minor release](https://doc.qubes-os.org/en/latest/developer/releases/version-scheme.html). This means that the EOL date for Qubes 4.2 was set at the time Qubes 4.3 was released by adding six months to the Qubes 4.3 release date. Qubes 4.3.0 was [released on 2025-12-21](https://www.qubes-os.org/news/2025/12/21/qubes-os-4-3-0-has-been-released/). Adding six months to this date gives us 2026-06-21, which is Qubes 4.2's EOL date. Since the EOL date of 4.2 was determined at the time 4.3 was released, we also [included this information in the 4.3.0 release announcement](https://www.qubes-os.org/news/2025/12/21/qubes-os-4-3-0-has-been-released/#support-for-older-releases).
