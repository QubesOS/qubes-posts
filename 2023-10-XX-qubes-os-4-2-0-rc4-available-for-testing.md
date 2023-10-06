---
layout: post
title: "Qubes OS 4.2.0-rc4 is available for testing"
categories: releases
download_url: /downloads/
---

We're pleased to announce that the fourth [release candidate (RC)](#what-is-a-release-candidate) for Qubes OS 4.2.0 is now available for [testing](/doc/testing/). The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## Main changes from RC3 to RC4

- Fixed: ["Qube Manager freezes while opening settings" (#8387)](https://github.com/QubesOS/qubes-issues/issues/8387)
- Fixed: ["Error when attempting to update dom0 in the Qube Manager" (#8117)](https://github.com/QubesOS/qubes-issues/issues/8117)
- Fixed: ["XScreenSaver & XScreenSaver Settings not opening window" (#8266)](https://github.com/QubesOS/qubes-issues/issues/8266)
- Fixed: ["Setting no-strict-reset option via salt on already attached devices doesn't work" (#8514)](https://github.com/QubesOS/qubes-issues/issues/8514)
- Fixed: ["qubes-video-companion-receiver missing dependency on acl package" (#8426)](https://github.com/QubesOS/qubes-issues/issues/8426)
- Fixed: ["OpenBSD 7.3 ISO doesn't boot anymore" (#8502)](https://github.com/QubesOS/qubes-issues/issues/8502)
- Fixed: ["Kernel compile bogs down rest of system" (#8176)](https://github.com/QubesOS/qubes-issues/issues/8176)

For an overview of major changes from Qubes 4.1 to 4.2, please see the [Qubes OS 4.2.0 release notes](/doc/releases/4.2/release-notes/).

## When is the stable release?

That depends on the number of bugs discovered in this RC and their severity. As explained in our [release schedule](/doc/version-scheme/#release-schedule) documentation, our usual process after issuing a new RC is to collect bug reports, triage the bugs, and fix them. This usually takes around five weeks, depending on the bugs discovered. If warranted, we then issue a new RC that includes the fixes and repeat the whole process again. We continue this iterative procedure until we're left with an RC that's good enough to be declared the stable release. No one can predict, at the outset, how many iterations will be required (and hence how many RCs will be needed before a stable release), but we tend to get a clearer picture of this with each successive RC, which we share in this section in each RC announcement. Here is the latest update:

At this point, we are hopeful that RC4 will be the final RC.

## Testing Qubes 4.2.0-rc4

Thank you to everyone who tested the previous Qubes 4.2.0 RCs! Due to your efforts, this new RC includes fixes for several bugs that were present in the previous RCs.

If you're willing to [test](/doc/testing/) this new RC, you can help us improve the eventual stable release by [reporting any bugs you encounter](/doc/issue-tracking/). We encourage experienced users to join the [testing team](https://forum.qubes-os.org/t/joining-the-testing-team/5190).

A full list of issues affecting Qubes 4.2.0 is available [here](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue+label%3Aaffects-4.2). We strongly recommend [updating Qubes OS](/doc/how-to-update/) immediately after installation in order to apply all available bug fixes.

## Upgrading to Qubes 4.2.0-rc4

If you're currently running any Qubes 4.2.0 RC, you can upgrade to the latest RC by [enabling the `current-testing` repo in dom0](/doc/how-to-install-software-in-dom0/#testing-repositories), then [updating normally](/doc/how-to-update/). However, please note that there have been some recent template changes, which are detailed in the [Qubes OS 4.2.0 release notes](/doc/releases/4.2/release-notes/).

If you're currently on Qubes 4.1 and wish to test 4.2, please see [how to upgrade to Qubes 4.2](/doc/upgrade/4.2/), which details both clean installation and in-place upgrade options. As always, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand.

## Reminder: new signing key for Qubes OS 4.2

As a reminder, we published the following special announcement in [Qubes Canary 032](/news/2022/09/14/canary-032/) on 2022-09-14:

> We plan to create a new Release Signing Key (RSK) for Qubes OS 4.2. Normally, we have only one RSK for each major release. However, for the 4.2 release, we will be using Qubes Builder version 2, which is a complete rewrite of the Qubes Builder. Out of an abundance of caution, we would like to isolate the build processes of the current stable 4.1 release and the upcoming 4.2 release from each other at the cryptographic level in order to minimize the risk of a vulnerability in one affecting the other. We are including this notice as a canary special announcement since introducing a new RSK for a minor release is an exception to our usual RSK management policy.

As always, we encourage you to [authenticate](/security/pack/#how-to-obtain-and-authenticate) this canary by [verifying its PGP signatures](/security/verifying-signatures/). Specific instructions are also included in the [canary announcement](/news/2022/09/14/canary-032/).

As with all Qubes signing keys, we also encourage you to [authenticate](/security/verifying-signatures/#how-to-import-and-authenticate-release-signing-keys) the new Qubes OS Release 4.2 Signing Key, which is available in the [Qubes Security Pack (qubes-secpack)](/security/pack/) as well as on the [downloads](/downloads/) page under the Qubes OS 4.2.0-rc4 ISO.

## What is a release candidate?

A release candidate (RC) is a software build that has the potential to become a stable release, unless significant bugs are discovered in testing. RCs are intended for more advanced (or adventurous!) users who are comfortable testing early versions of software that are potentially buggier than stable releases. You can read more about Qubes OS [supported releases](/doc/supported-releases/) and the [version scheme](/doc/version-scheme/) in our documentation.
