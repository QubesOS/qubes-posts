---
layout: post
title: "Qubes OS 4.3.0 has been released!"
categories: releases
download_url: /downloads/
---

We're pleased to announce the stable release of Qubes OS 4.3.0! This minor release includes a host of new features, improvements, and bug fixes. The ISO and associated [verification files](https://doc.qubes-os.org/en/latest/project-security/verifying-signatures.html) are available on the [downloads](https://www.qubes-os.org/downloads/) page.

## What's new in Qubes 4.3?

- Dom0 upgraded to Fedora 41 ([#9402](https://github.com/QubesOS/qubes-issues/issues/9402)).
- Xen upgraded to version 4.19 ([#9420](https://github.com/QubesOS/qubes-issues/issues/9420)).
- Default Fedora template upgraded to Fedora 42 (older versions not supported).
- Default Debian template upgraded to Debian 13 (versions older than 12 not supported).
- Default Whonix templates upgraded to Whonix 18 (older versions not supported).
- Preloaded disposables ([#1512](https://github.com/QubesOS/qubes-issues/issues/1512))
- Device "self-identity oriented" assignment (a.k.a. New Devices API) ([#9325](https://github.com/QubesOS/qubes-issues/issues/9325))
- Qubes Windows Tools reintroduced with improved features ([#1861](https://github.com/QubesOS/qubes-issues/issues/1861)).

These are just a few highlights from the many changes included in this release. For a more comprehensive list of changes, see the [Qubes OS 4.3 release notes](https://doc.qubes-os.org/en/latest/developer/releases/4_3/release-notes.html).

## How to get Qubes OS 4.3.0

- If you'd like to install Qubes OS for the first time or perform a clean reinstallation on an existing system, there's never been a better time to do so! Simply [download](https://www.qubes-os.org/downloads/) the Qubes 4.3.0 ISO and follow our [installation guide](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/installation-guide.html).

- If you're currently using Qubes 4.2, learn [how to upgrade to Qubes 4.3](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_3.html).

- If you're currently using a Qubes 4.3 release candidate (RC), [update normally](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html) (which includes [upgrading any EOL templates and standalones](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html#upgrading-to-avoid-eol) you might have) in order to make your system effectively equivalent to the stable Qubes 4.3.0 release. No reinstallation or other special action is required.

In all cases, we strongly recommend [making a full backup](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html) beforehand.

## Known issues in Qubes OS 4.3.0

Templates restored in 4.3.0 from a pre-4.3 backup may continue to target their original Qubes OS release repos ([#8701](https://github.com/QubesOS/qubes-issues/issues/8701)). After restoring such templates in 4.3.0, you must perform the following additional steps:

```
sudo qubes-dom0-update -y qubes-dist-upgrade
qubes-dist-upgrade --releasever=4.3 --template-standalone-upgrade -y
```

This will automatically choose the templates that need to be updated.

Fresh templates on a clean 4.3.0 installation are not affected. Users who perform an in-place upgrade from 4.2 to 4.3 (instead of restoring templates from a backup) are also not affected, since the in-place upgrade process already includes the above fix in stage 4. For more information, see issue [#8701](https://github.com/QubesOS/qubes-issues/issues/8701).

[View the full list of known bugs affecting Qubes 4.3](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue%20type%3ABug%20label%3Aaffects-4.3%20-label%3A%22R%3A%20cannot%20reproduce%22%20-label%3A%22R%3A%20declined%22%20-label%3A%22R%3A%20duplicate%22%20-label%3A%22R%3A%20not%20applicable%22%20-label%3A%22R%3A%20self-closed%22%20-label%3A%22R%3A%20upstream%20issue%22) in our [issue tracker](https://doc.qubes-os.org/en/latest/introduction/issue-tracking.html).

## Support for older releases

In accordance with our [release support policy](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html#qubes-os), Qubes 4.2 will remain supported for six months after the release of Qubes 4.3, until 2026-06-[TODO]. After that, Qubes 4.2 will no longer receive security updates or bug fixes.

[Whonix templates](https://www.whonix.org/wiki/Qubes) are created and supported by our partner, the [Whonix Project](https://www.whonix.org/). The Whonix Project has set its own support policy for Whonix templates in Qubes. For more information, see the [Whonix Support Schedule](https://www.whonix.org/wiki/About#Support_Schedule).

## Thank you to our partners, donors, contributors, and testers!

This release would not be possible without generous support from our [partners](https://www.qubes-os.org/partners/) and [donors](https://www.qubes-os.org/donate/), as well as [contributions](https://doc.qubes-os.org/en/latest/introduction/contributing.html) from our active community members, especially [bug reports](https://doc.qubes-os.org/en/latest/introduction/issue-tracking.html) from our [testers](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/testing.html). We are eternally grateful to our excellent community for making the Qubes OS Project a great example of open-source collaboration.
