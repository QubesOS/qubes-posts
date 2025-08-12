---
layout: post
title: "Qubes OS 4.2.0-rc1 is available for testing"
categories: releases
download_url: /downloads/
---

We're pleased to announce that the first [release candidate](#what-is-a-release-candidate) for Qubes OS 4.2.0 is now available for [testing](/doc/testing/). This [minor release](#what-is-a-minor-release) includes several new features and improvements over Qubes OS 4.1.0. Qubes 4.2.0-rc1 is available on the [downloads](/downloads/) page.


## What's new in Qubes 4.2.0?

- Dom0 upgraded to Fedora 37
- Xen updated to version 4.17
- SELinux support in Fedora templates
- Several GUI applications rewritten, including:
  - Applications Menu
  - Qubes Global Settings
  - Create New Qube
  - Qubes Update
- Unified `grub.cfg` location for both UEFI and legacy boot
- PipeWire support
- fwupd integration for firmware updates
- Optional automatic clipboard clearing
- Official packages built using Qubes Builder v2
- Split GPG management in Qubes Global Settings

Please see the [Qubes OS 4.2.0 release notes](/doc/releases/4.2/release-notes/) for details.

## Reminder: new signing key for Qubes OS 4.2

As a reminder, we published the following special announcement in [Qubes Canary 032](/news/2022/09/14/canary-032/) on 2022-09-14:

> We plan to create a new Release Signing Key (RSK) for Qubes OS 4.2. Normally, we have only one RSK for each major release. However, for the 4.2 release, we will be using Qubes Builder version 2, which is a complete rewrite of the Qubes Builder. Out of an abundance of caution, we would like to isolate the build processes of the current stable 4.1 release and the upcoming 4.2 release from each other at the cryptographic level in order to minimize the risk of a vulnerability in one affecting the other. We are including this notice as a canary special announcement since introducing a new RSK for a minor release is an exception to our usual RSK management policy.

As always, we encourage you to [authenticate](/security/pack/#how-to-obtain-and-authenticate) this canary by [verifying its PGP signatures](/security/verifying-signatures/). Specific instructions are also included in the [canary announcement](/news/2022/09/14/canary-032/).

As with all Qubes signing keys, we also encourage you to [authenticate](/security/verifying-signatures/#how-to-import-and-authenticate-release-signing-keys) the new Qubes OS Release 4.2 Signing Key, which is available in the [Qubes Security Pack (qubes-secpack)](/security/pack/) as well as on the [downloads](/downloads/) page under the Qubes OS 4.2.0-rc1 ISO.

## Testing Qubes 4.2.0-rc1

If you're willing to [test](/doc/testing/) this release candidate, you can help us improve the eventual stable release by [reporting any bugs you encounter](/doc/issue-tracking/). We encourage experienced users to join the [testing team](https://forum.qubes-os.org/t/joining-the-testing-team/5190).

A full list of known bugs in Qubes 4.2.0 is available [here](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Release+4.2%22+label%3A%22T%3A+bug%22). We strongly recommend [updating Qubes OS](/doc/how-to-update/) immediately after installation in order to apply all available bug fixes.

## Upgrading to Qubes 4.2.0-rc1

It is not yet possible to perform an in-place upgrade from Qubes 4.1 to Qubes 4.2. For this initial release candidate, a clean installation is required. An in-place upgrade tool is in development.

## When is the stable release?

That depends on the number of bugs discovered in this release candidate and their severity. As explained in our [release schedule](/doc/version-scheme/#release-schedule) documentation, our usual process after issuing a new release candidate is to collect bug reports, triage the bugs, and fix them. This usually takes around five weeks, depending on the bugs discovered. If warranted, we then issue a new release candidate that includes the fixes and repeat the whole process again. We continue this iterative procedure until we're left with a release candidate that's good enough to be declared the stable release. No one can predict, at the outset, how many iterations will be required (and hence how many release candidates will be needed before a stable release), but we tend to get a clearer picture of this with each successive release candidate, which we'll share in this section in future release candidate announcements.

In the case of Qubes 4.2.0 specifically, we already know that there will be a second release candidate (in order to test the in-place upgrade procedure, if nothing else). As mentioned above, we expect to announce that second release candidate in approximately five weeks. The results of that second release candidate will determine whether a third one is required.

## What is a release candidate?

A release candidate (RC) is a software build that has the potential to become a stable release, unless significant bugs are discovered in testing. Release candidates are intended for more advanced (or adventurous!) users who are comfortable testing early versions of software that are potentially buggier than stable releases. You can read more about Qubes OS [supported releases](/doc/supported-releases/) and the [version scheme](/doc/version-scheme/) in our documentation.

## What is a minor release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. Hence, releases that increment the second value are known as "minor releases." Minor releases generally include new features, improvements, and bug fixes that are backward-compatible with earlier versions of the same major release. See our [supported releases](/doc/supported-releases/) for a comprehensive list of major and minor releases and our [version scheme](/doc/version-scheme/) documentation for more information about how Qubes OS releases are versioned.
