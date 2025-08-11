---
layout: post
title: "Qubes OS 4.3.0-rc1 is available for testing"
categories: releases
download_url: /downloads/
---

We're pleased to announce that the first release candidate (RC) for Qubes OS 4.3.0 is now available for [testing](/doc/testing/). This minor release includes many new features and improvements over Qubes OS 4.2. The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## What's new in Qubes 4.3.0?

- Dom0 upgraded to Fedora 41 ([#9402](https://github.com/QubesOS/qubes-issues/issues/9402)).
- Xen upgraded to version 4.19 ([#9420](https://github.com/QubesOS/qubes-issues/issues/9420)).
- Default Fedora template upgraded to Fedora 42 (older releases not supported).
- Default Debian template upgraded to Debian 13 (older releases not supported).
- Default Whonix templates upgraded to Whonix 17.4.3 (older releases not supported).
- Preloaded disposables ([#1512](https://github.com/QubesOS/qubes-issues/issues/1512)
- Device "self-identity oriented" assignment (a.k.a. New Devices API) ([#9325](https://github.com/QubesOS/qubes-issues/issues/9325))
- QWT (Qubes Windows Tools) reintroduced with improved features ([#1861](github.com/QubesOS/qubes-issues/issues/1861)).

These are just a few highlights of the many changes included in this release. For a more comprehensive list of changes, see the [Qubes OS 4.3.0 release notes](https://qubes-doc--1504.org.readthedocs.build/en/1504/developer/releases/4_3/release-notes.html). (**Note:** This is a temporary link to an early preview of the release notes, which are continually being updated as we progress toward the eventual stable release.)

## When is the stable release?

That depends on the number of bugs discovered in this RC and their severity. As explained in our [release schedule](/doc/version-scheme/#release-schedule) documentation, our usual process after issuing a new RC is to collect bug reports, triage the bugs, and fix them. If warranted, we then issue a new RC that includes the fixes and repeat the process. We continue this iterative procedure until we're left with an RC that's good enough to be declared the stable release. No one can predict, at the outset, how many iterations will be required (and hence how many RCs will be needed before a stable release), but we tend to get a clearer picture of this as testing progresses.

## How to test Qubes 4.3.0-rc1

If you're willing to [test](/doc/testing/) this release candidate, you can help us improve the eventual stable release by [reporting any bugs you encounter](/doc/issue-tracking/). You can [upgrade to Qubes 4.3.0-rc1](https://qubes-doc--1504.org.readthedocs.build/en/1504/user/downloading-installing-upgrading/upgrade/4_3.html) with either a clean installation or an in-place upgrade from Qubes 4.2. (Note for in-place upgrade testers: `qubes-dist-upgrade` now requires `--releasever=4.3` and may require `--enable-current-testing` for testing releases.) As always, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand and [updating Qubes OS](/doc/how-to-update/) immediately afterward in order to apply all available bug fixes. We encourage experienced users to join the [testing team](https://forum.qubes-os.org/t/joining-the-testing-team/5190). 

## Known issues in Qubes OS 4.3.0-rc1

Templates restored in 4.3 from a pre-4.3 backup continue to target their original Qubes OS release repos. This does not affect fresh templates on a clean 4.3.0-rc1 installation. For more information, see issue [#8701](https://github.com/QubesOS/qubes-issues/issues/8701).

[View the full list of known bugs affecting Qubes 4.3](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue%20type%3ABug%20label%3Aaffects-4.3%20-label%3A%22R%3A%20cannot%20reproduce%22%20-label%3A%22R%3A%20declined%22%20-label%3A%22R%3A%20duplicate%22%20-label%3A%22R%3A%20not%20applicable%22%20-label%3A%22R%3A%20self-closed%22%20-label%3A%22R%3A%20upstream%20issue%22) in our [issue tracker](/doc/issue-tracking/).

## What's a release candidate?

A release candidate (RC) is a software build that has the potential to become a stable release, unless significant bugs are discovered in testing. RCs are intended for more advanced (or adventurous!) users who are comfortable testing early versions of software that are potentially buggier than stable releases. You can read more about Qubes OS [supported releases](/doc/supported-releases/) and the [version scheme](/doc/version-scheme/) in our documentation.

## What's a minor release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `<major>.<minor>.<patch>`. Hence, releases that increment the second value are known as "minor releases." Minor releases generally include new features, improvements, and bug fixes that are backward-compatible with earlier versions of the same major release. See our [supported releases](/doc/supported-releases/) for a comprehensive list of major and minor releases and our [version scheme](/doc/version-scheme/) documentation for more information about how Qubes OS releases are versioned.
