---
layout: post
title: "Qubes OS 4.2 approaching end of life"
categories:
  - announcements
  - releases
---

Qubes OS 4.2 is scheduled to reach end-of-life (EOL) on 2026-06-21, approximately two months from the date of this announcement.

## Recommended actions

If you're currently using Qubes 4.2, you should [upgrade to Qubes 4.3](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html) no later than 2026-06-21. There are two ways to upgrade:

- [Perform a clean installation of Qubes 4.3.](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html#clean-installation-of-qubes-4-3) This involves backing up your current system, replacing your current installation with a fresh Qubes 4.3 installation, then restoring from your backup. Many users find this option to be simpler, easier, and less error-prone. However, if you've made extensive customizations in dom0, they may need to be redone.

- [Perform an in-place upgrade from Qubes 4.2 to Qubes 4.3.](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html#in-place-upgrade-from-qubes-4-2-to-qubes-4-3) Instead of replacing your existing installation, this method involves installing a special command-line tool in dom0, then using it to upgrade your existing Qubes 4.2 installation to Qubes 4.3. This is a more complex multi-stage process, which makes it most suitable for advanced users. This method preserves your qubes and all the customizations you've made in dom0 that are compatible with the in-place upgrade process. While not strictly required, we still strongly recommend [making a full backup](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html) before attempting an in-place upgrade. This way, if anything goes wrong, your data is still safe, and you always have the option of falling back to performing a clean installation.

(If you're already on Qubes 4.3, then you don't have to do anything. This announcement doesn't apply to you.)

## What does end-of-life (EOL) mean?

When a Qubes OS release reaches end-of-life (EOL), it's no longer supported. This means that bugs discovered in that release will no longer be fixed, and enhancements will no longer be added. Most importantly, releases that have reached EOL no longer receive security updates, which is why it's critically important to upgrade to a supported release.

## What about patch releases?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. When a major or minor release reaches EOL, all of its patch releases also reach EOL. For example, when we say that "Qubes 4.2" (without specifying a `[patch]` number) is approaching EOL, we're specifying a particular minor release, inclusive of all patch releases within it. This means that Qubes 4.2.0, 4.2.1, 4.2.2, 4.2.3, and 4.2.4 will all reach EOL at the same time (on 2026-06-21), since they're all patch releases of the same minor release.

## How are EOL dates determined?

According to our [support policy](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#qubes-os), stable Qubes OS releases are supported for six months after each subsequent [major or minor release](https://doc.qubes-os.org/en/latest/developer/releases/version-scheme.html). This means that Qubes 4.2 reaches EOL six months after Qubes 4.3 was released. Since Qubes 4.3.0 was [released on 2025-12-21](https://www.qubes-os.org/news/2025/12/21/qubes-os-4-3-0-has-been-released/), Qubes 4.2's EOL date is six months later, which is 2026-06-21.
