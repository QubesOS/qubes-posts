---
layout: post
title: "Qubes OS 4.3.0-rc1 is available for testing"
categories: releases
download_url: /downloads/
---

We're pleased to announce that the first release candidate (RC) for Qubes OS 4.3.0 is now available for testing. This minor release includes several new features and improvements over Qubes OS 4.2. The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## What's new in Qubes 4.3.0?

- TODO

For more information about the changes included in this version, see the [Qubes OS 4.3.0 release notes](/doc/releases/4.3/release-notes/).

## When is the stable release?

That depends on the number of bugs discovered in this RC and their severity. As explained in our [release schedule](/doc/version-scheme/#release-schedule) documentation, our usual process after issuing a new RC is to collect bug reports, triage the bugs, and fix them. If warranted, we then issue a new RC that includes the fixes and repeat the process. We continue this iterative procedure until we're left with an RC that's good enough to be declared the stable release. No one can predict, at the outset, how many iterations will be required (and hence how many RCs will be needed before a stable release), but we tend to get a clearer picture of this as testing progresses.

## Testing Qubes 4.3.0-rc1

If you're willing to [test](/doc/testing/) this new RC, you can help us improve the eventual stable release by [reporting any bugs you encounter](/doc/issue-tracking/). We encourage experienced users to join the [testing team](https://forum.qubes-os.org/t/joining-the-testing-team/5190). The best way to test Qubes 4.3.0-rc1 is by performing a [clean installation](/doc/installation-guide/) with the new ISO. We strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand. (Please note that [in-place upgrades from 4.2 to 4.3](https://github.com/QubesOS/qubes-issues/issues/9317) have not yet been implemented as of RC1.)

A full list of known bugs affecting Qubes 4.3 is available [here](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue%20type%3ABug%20label%3Aaffects-4.3%20-label%3A%22R%3A%20cannot%20reproduce%22%20-label%3A%22R%3A%20declined%22%20-label%3A%22R%3A%20duplicate%22%20-label%3A%22R%3A%20not%20applicable%22%20-label%3A%22R%3A%20self-closed%22%20-label%3A%22R%3A%20upstream%20issue%22). We strongly recommend [updating Qubes OS](/doc/how-to-update/) immediately after installation in order to apply all available bug fixes.

## What is a release candidate?

A release candidate (RC) is a software build that has the potential to become a stable release, unless significant bugs are discovered in testing. RCs are intended for more advanced (or adventurous!) users who are comfortable testing early versions of software that are potentially buggier than stable releases. You can read more about Qubes OS [supported releases](/doc/supported-releases/) and the [version scheme](/doc/version-scheme/) in our documentation.

## What is a minor release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `<major>.<minor>.<patch>`. Hence, releases that increment the second value are known as "minor releases." Minor releases generally include new features, improvements, and bug fixes that are backward-compatible with earlier versions of the same major release. See our [supported releases](/doc/supported-releases/) for a comprehensive list of major and minor releases and our [version scheme](/doc/version-scheme/) documentation for more information about how Qubes OS releases are versioned.
