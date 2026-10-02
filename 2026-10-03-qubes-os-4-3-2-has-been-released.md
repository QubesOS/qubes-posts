---
layout: post
title: "Qubes OS 4.3.2 has been released!"
categories: releases
download_url: /downloads/
---

We're pleased to announce the stable release of Qubes OS 4.3.2! This patch release aims to consolidate all the security updates and bug fixes that have occurred since the previous stable release. Our goal is to provide a secure and convenient way for users to install (or reinstall) the latest stable Qubes release with an up-to-date ISO. The ISO and associated [verification files](https://doc.qubes-os.org/en/latest/project-security/verifying-signatures.html) are available on the [downloads](https://www.qubes-os.org/downloads/) page.

## Announcements

- [Qubes OS Summit 2026](https://pretix.eu/qubes/summit2026/) is coming up soon! This year's Summit will take place from October 30 to November 1 in Berlin.

- As a reminder, [Qubes 4.2 has reached end of life (EOL)](https://www.qubes-os.org/news/2026/06/21/qubes-os-4-2-has-reached-end-of-life/). If you're still using Qubes 4.2, we urge you to [upgrade to Qubes 4.3](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html) immediately.

## What's new in Qubes 4.3.2?

- Default Fedora template upgraded to Fedora 44
- `kernel-latest` upgraded to Linux 7.2
- [Security updates](https://www.qubes-os.org/security/qsb/)
- [Numerous bug fixes](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue%20is%3Aclosed%20reason%3Acompleted%20type%3ABug%20label%3A%22affects-4.3%22%20closed%3A2026-06-11..2026-09-18%20-label%3A%22R%3A%20cannot%20reproduce%22%20-label%3A%22R%3A%20declined%22%20-label%3A%22R%3A%20duplicate%22%20-label%3A%22R%3A%20not%20applicable%22%20-label%3A%22R%3A%20self-closed%22%20-label%3A%22R%3A%20upstream%20issue%22%20-label%3A%22C%3A%20website%22%20-label%3A%22C%3A%20infrastructure%22%20-label%3A%22C%3A%20tests%22)

For an overview of what's new in Qubes 4.3, see the [Qubes 4.3 release notes](https://doc.qubes-os.org/en/r4.3/developer/releases/4_3/release-notes.html).

## How to get Qubes 4.3.2

- If you'd like to install Qubes for the first time or perform a clean reinstallation on an existing system, there's never been a better time to do so! Simply [download](https://www.qubes-os.org/downloads/) the Qubes 4.3.2 ISO and follow our [installation guide](https://doc.qubes-os.org/en/r4.3/user/downloading-installing-upgrading/installation-guide.html).

- If you're currently on Qubes 4.3 (including 4.3.0, 4.3.1, and 4.3.2-rc1), [update normally](https://doc.qubes-os.org/en/r4.3/user/how-to-guides/how-to-update.html) (which includes [upgrading any EOL templates and standalones](https://doc.qubes-os.org/en/r4.3/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol) you might have) in order to make your system essentially equivalent to the stable Qubes 4.3.2 release. No reinstallation or other special action is required.

- If you're still using Qubes 4.2, you should [upgrade from 4.2 to 4.3](https://doc.qubes-os.org/en/r4.2/user/downloading-installing-upgrading/upgrade/4_3.html) immediately, since [Qubes 4.2 has already reached end of life (EOL)](https://www.qubes-os.org/news/2026/06/21/qubes-os-4-2-has-reached-end-of-life/).

In all cases, we strongly recommend [making a full backup](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html) beforehand.

## Known issues in Qubes 4.3.2

It's possible that templates restored in 4.3.2 from a pre-4.3 backup may continue to target their original Qubes OS release repos ([#8701](https://github.com/QubesOS/qubes-issues/issues/8701)). After restoring such templates in 4.3.2, enter the following additional commands in a dom0 terminal:

```
sudo qubes-dom0-update -y qubes-dist-upgrade
sudo qubes-dist-upgrade --releasever=4.3 --template-standalone-upgrade -y
```

This will automatically choose the templates that need to be upgraded. The templates will be shut down during this process.

Fresh templates on a clean 4.3.2 installation are not affected. Users who perform an in-place upgrade from 4.2 to 4.3 (instead of restoring templates from a backup) are also not affected, since the in-place upgrade process already includes the above fix in stage 4. For more information, see issue [#8701](https://github.com/QubesOS/qubes-issues/issues/8701).

[View the full list of known bugs affecting Qubes 4.3](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue%20state%3Aopen%20type%3ABug%20label%3Aaffects-4.3%20-label%3A%22R%3A%20cannot%20reproduce%22%20-label%3A%22R%3A%20declined%22%20-label%3A%22R%3A%20duplicate%22%20-label%3A%22R%3A%20not%20applicable%22%20-label%3A%22R%3A%20self-closed%22%20-label%3A%22R%3A%20upstream%20issue%22%20-label%3A%22C%3A%20website%22%20-label%3A%22C%3A%20infrastructure%22%20-label%3A%22C%3A%20tests%22) in our [issue tracker](https://doc.qubes-os.org/en/latest/introduction/issue-tracking.html).

## What's a patch release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. Hence, we refer to releases that increment the third number as "patch releases." A patch release does not designate a separate, new major or minor release of Qubes OS. Rather, it designates its respective major or minor release (in this case, 4.3) inclusive of all updates up to a certain point. See our [supported releases](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html) for a comprehensive list of major and minor releases and our [version scheme](https://doc.qubes-os.org/en/latest/developer/releases/version-scheme.html) documentation for more information about how Qubes OS releases are versioned.