---
layout: post
title: "Qubes OS 4.2.3-rc1 is available for testing"
categories: releases
download_url: /downloads/
---

We're pleased to announce that the first release candidate (RC) for Qubes OS 4.2.3 is now available for testing. This patch release aims to consolidate all the security patches, bug fixes, and other updates that have occurred since the previous stable release. Our goal is to provide a secure and convenient way for users to install (or reinstall) the latest stable Qubes release with an up-to-date ISO. The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## What's new in Qubes 4.2.3?

- All security updates to date
- All bug fixes to date

For more information about the changes included in this version, see the [Qubes OS 4.2 release notes](/doc/releases/4.2/release-notes/) and the [full list of issues completed since the previous stable release](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue+is%3Aclosed+reason%3Acompleted+closed%3A2024-03-26..2024-09-09+-label%3A%22R%3A+cannot+reproduce%22+-label%3A%22R%3A+declined%22+-label%3A%22R%3A+duplicate%22+-label%3A%22R%3A+not+applicable%22+-label%3A%22R%3A+self-closed%22+-label%3A%22R%3A+upstream+issue%22).

## When is the stable release?

That depends on the number of bugs discovered in this RC and their severity. As explained in our [release schedule](/doc/version-scheme/#release-schedule) documentation, our usual process after issuing a new RC is to collect bug reports, triage the bugs, and fix them. If warranted, we then issue a new RC that includes the fixes and repeat the process. We continue this iterative procedure until we're left with an RC that's good enough to be declared the stable release. No one can predict, at the outset, how many iterations will be required (and hence how many RCs will be needed before a stable release), but we tend to get a clearer picture of this as testing progresses.

## Testing Qubes 4.2.3-rc1

If you're willing to [test](/doc/testing/) this new RC, you can help us improve the eventual stable release by [reporting any bugs you encounter](/doc/issue-tracking/). We encourage experienced users to join the [testing team](https://forum.qubes-os.org/t/joining-the-testing-team/5190). The best way to test Qubes 4.2.3-rc1 is by performing a [clean installation](/doc/installation-guide/) with the new ISO. We strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand.

As an alternative to a clean installation, there is also the option of performing an in-place upgrade without reinstalling. However, since Qubes 4.2.3 is simply Qubes 4.2 inclusive of all updates to date, this amounts to simply using a fully-updated 4.2 installation. In a sense, then, all current 4.2 users who are keeping up with updates are already testing 4.2.3-rc1, but this testing is only partial, since it does not cover things like the installation procedure. 

## Reminder: new signing key for Qubes 4.2

As a reminder, we published the following special announcement in [Qubes Canary 032](/news/2022/09/14/canary-032/) on 2022-09-14:

> We plan to create a new Release Signing Key (RSK) for Qubes OS 4.2. Normally, we have only one RSK for each major release. However, for the 4.2 release, we will be using Qubes Builder version 2, which is a complete rewrite of the Qubes Builder. Out of an abundance of caution, we would like to isolate the build processes of the current stable 4.1 release and the upcoming 4.2 release from each other at the cryptographic level in order to minimize the risk of a vulnerability in one affecting the other. We are including this notice as a canary special announcement since introducing a new RSK for a minor release is an exception to our usual RSK management policy.

As always, we encourage you to [authenticate](/security/pack/#how-to-obtain-and-authenticate) this canary by [verifying its PGP signatures](/security/verifying-signatures/). Specific instructions are also included in the [canary announcement](/news/2022/09/14/canary-032/).

As with all Qubes signing keys, we also encourage you to [authenticate](/security/verifying-signatures/#how-to-import-and-authenticate-release-signing-keys) the Qubes OS Release 4.2 Signing Key, which is available in the [Qubes Security Pack (qubes-secpack)](/security/pack/) as well as on the [downloads](/downloads/) page.

## What is a release candidate?

A release candidate (RC) is a software build that has the potential to become a stable release, unless significant bugs are discovered in testing. RCs are intended for more advanced (or adventurous!) users who are comfortable testing early versions of software that are potentially buggier than stable releases. You can read more about Qubes OS [supported releases](/doc/supported-releases/) and the [version scheme](/doc/version-scheme/) in our documentation.

## What is a patch release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. Hence, we refer to releases that increment the third number as "patch releases." A patch release does not designate a separate, new major or minor release of Qubes OS. Rather, it designates its respective major or minor release (in this case, 4.2) inclusive of all updates up to a certain point. (See [supported releases](/doc/supported-releases/) for a comprehensive list of major and minor releases.) Installing the initial Qubes 4.2.0 release and fully [updating](/doc/how-to-update/) it results in essentially the same system as installing Qubes 4.2.3. You can learn more about how Qubes release versioning works in the [version scheme](/doc/version-scheme/) documentation.
