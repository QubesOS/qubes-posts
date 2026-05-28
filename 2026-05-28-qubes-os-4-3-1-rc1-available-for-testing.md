---
layout: post
title: "Qubes OS 4.3.1-rc1 is available for testing"
categories: releases
download_url: /downloads/
---

The first release candidate (RC) for Qubes OS 4.3.1 is now available for testing. This patch release aims to consolidate all the security patches, bug fixes, and other updates that have occurred since the release of Qubes 4.3.0.

## What's new in Qubes 4.3.1?

- Default Fedora template upgraded to Fedora 43. (Note: [Fedora 42 has reached end of life.](https://www.qubes-os.org/news/2026/03/13/fedora-42-approaching-end-of-life/))
- Many bugs fixes

## When is the stable release?

That depends on the number of bugs discovered in this RC and their severity. As explained in our [release schedule](https://doc.qubes-os.org/en/latest/developer/releases/version-scheme.html#release-schedule) documentation, our usual process after issuing a new RC is to collect bug reports, triage the bugs, and fix them. If warranted, we then issue a new RC that includes the fixes and repeat the process. We continue this iterative procedure until we're left with an RC that's good enough to be declared the stable release. No one can predict with certainty, at the outset, how many iterations will be required (and hence how many RCs will be needed before a stable release), but we tend to get a clearer picture of this as testing progresses.

Since the changes between 4.3.0 and 4.3.1 are relatively minor, we currently don't anticipate any major problems requiring a second RC. We currently expect to be able to publish the stable 4.3.1 release in one to two weeks.

## How to test Qubes 4.3.1-rc1

If you'd like to help us [test](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/testing.html) this RC, the best way to do so is by performing a [clean installation](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/installation-guide.html) with [the new ISO](https://www.qubes-os.org/downloads/). As always, we strongly recommend [making a full backup](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-back-up-restore-and-migrate.html) beforehand and [updating Qubes OS](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-update.html) immediately afterward in order to apply all available bug fixes.

As an alternative to a clean installation, there's also the option of performing an in-place upgrade without reinstalling. However, since Qubes 4.3.1 is a patch release, it's essentially Qubes 4.3.0 inclusive of all updates to date, which largely amounts to just using a fully-updated 4.3.0 installation. By contrast, a clean installation covers other areas that could also benefit from testing, such as the installation procedure, which is why it's the recommended testing method.

Regardless of your testing method, please help us improve the eventual stable release by [reporting any bugs you encounter](https://doc.qubes-os.org/en/latest/introduction/issue-tracking.html). If you're an experienced user, we encourage you to [join the testing team](https://forum.qubes-os.org/t/joining-the-testing-team/5190).

## Known issues in Qubes OS 4.3.1

It is possible that templates restored in 4.3.1 from a pre-4.3 backup may continue to target their original Qubes OS release repos. This does not affect fresh templates on a clean 4.3.1 installation. For more information, see issue [#8701](https://github.com/QubesOS/qubes-issues/issues/8701).

[View the full list of known bugs affecting Qubes 4.3](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue%20type%3ABug%20label%3Aaffects-4.3%20-label%3A%22R%3A%20cannot%20reproduce%22%20-label%3A%22R%3A%20declined%22%20-label%3A%22R%3A%20duplicate%22%20-label%3A%22R%3A%20not%20applicable%22%20-label%3A%22R%3A%20self-closed%22%20-label%3A%22R%3A%20upstream%20issue%22) in our [issue tracker](https://doc.qubes-os.org/en/latest/introduction/issue-tracking.html).

## What's a release candidate?

A release candidate (RC) is a software build that has the potential to become a stable release, unless significant bugs are discovered in testing. RCs are intended for more advanced (or adventurous!) users who are comfortable testing early versions of software that are potentially buggier than stable releases. You can read more about Qubes OS [supported releases](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html) and the [version scheme](https://doc.qubes-os.org/en/latest/developer/releases/version-scheme.html) in our documentation.

## What's a patch release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. Hence, we refer to releases that increment the third number as "patch releases." A patch release does not designate a separate, new major or minor release of Qubes OS. Rather, it designates its respective major or minor release (in this case, 4.3) inclusive of all updates up to a certain point. See our [supported releases](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/supported-releases.html) for a comprehensive list of major and minor releases and our [version scheme](https://doc.qubes-os.org/en/latest/developer/releases/version-scheme.html) documentation for more information about how Qubes OS releases are versioned.