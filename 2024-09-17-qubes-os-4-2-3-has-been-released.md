---
layout: post
title: "Qubes OS 4.2.3 has been released!"
categories: releases
download_url: /downloads/
---

We're pleased to announce the stable release of Qubes OS 4.2.3! This patch release aims to consolidate all the security patches, bug fixes, and other updates that have occurred since the previous stable release. Our goal is to provide a secure and convenient way for users to install (or reinstall) the latest stable Qubes release with an up-to-date ISO. The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## What's new in Qubes 4.2.3?

- All security updates to date
- All bug fixes to date

For more information about the changes included in this version, see the [Qubes OS 4.2 release notes](/doc/releases/4.2/release-notes/) and the [full list of issues completed since the previous stable release](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue+is%3Aclosed+reason%3Acompleted+closed%3A2024-03-26..2024-09-09+-label%3A%22R%3A+cannot+reproduce%22+-label%3A%22R%3A+declined%22+-label%3A%22R%3A+duplicate%22+-label%3A%22R%3A+not+applicable%22+-label%3A%22R%3A+self-closed%22+-label%3A%22R%3A+upstream+issue%22).

## How to get Qubes 4.2.3

You have a couple different options, depending on your situation:

- If you'd like to install Qubes OS for the first time or perform a clean reinstallation on an existing system, there's never been a better time to do so! Simply [download](/downloads/) the Qubes 4.2.3 ISO and follow our [installation guide](/doc/installation-guide/).

- If you're currently on Qubes 4.2 (including 4.2.0, 4.2.1, 4.2.2, and 4.2.3-rc1), [update normally](/doc/how-to-update/) (which includes [upgrading any EOL templates](/doc/how-to-update/#upgrading-to-avoid-eol) you might have) in order to make your system essentially equivalent to the stable Qubes 4.2.3 release. No reinstallation or other special action is required.

- Please note that [Qubes 4.1 has reached end-of-life](/news/2024/06/18/qubes-os-4-1-has-reached-end-of-life-extended-security-support-continues-until-2024-07-31/) and [extended security support for Qubes 4.1 has ended](/news/2024/08/01/extended-security-support-for-qubes-os-4-1-has-ended/). If you're still on Qubes 4.1 or an earlier release, you should [upgrade to Qubes 4.2 immediately](/doc/upgrade/4.2/).

In all cases, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand.

## Reminder: new signing key for Qubes 4.2

As a reminder for those upgrading from Qubes 4.1 and earlier, we published the following special announcement in [Qubes Canary 032](/news/2022/09/14/canary-032/) on 2022-09-14:

> We plan to create a new Release Signing Key (RSK) for Qubes OS 4.2. Normally, we have only one RSK for each major release. However, for the 4.2 release, we will be using Qubes Builder version 2, which is a complete rewrite of the Qubes Builder. Out of an abundance of caution, we would like to isolate the build processes of the current stable 4.1 release and the upcoming 4.2 release from each other at the cryptographic level in order to minimize the risk of a vulnerability in one affecting the other. We are including this notice as a canary special announcement since introducing a new RSK for a minor release is an exception to our usual RSK management policy.

As always, we encourage you to [authenticate](/security/pack/#how-to-obtain-and-authenticate) this canary by [verifying its PGP signatures](/security/verifying-signatures/). Specific instructions are also included in the [canary announcement](/news/2022/09/14/canary-032/).

As with all Qubes signing keys, we also encourage you to [authenticate](/security/verifying-signatures/#how-to-import-and-authenticate-release-signing-keys) the Qubes OS Release 4.2 Signing Key, which is available in the [Qubes Security Pack (qubes-secpack)](/security/pack/) as well as on the [downloads](/downloads/) page.

## What is a patch release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `<major>.<minor>.<patch>`. Hence, we refer to releases that increment the third number as "patch releases." A patch release does not designate a separate, new major or minor release of Qubes OS. Rather, it designates its respective major or minor release (in this case, 4.2) inclusive of all updates up to a certain point. (See [supported releases](/doc/supported-releases/) for a comprehensive list of major and minor releases.) Installing the initial Qubes 4.2.0 release and fully [updating](/doc/how-to-update/) it results in essentially the same system as installing Qubes 4.2.3. You can learn more about how Qubes release versioning works in the [version scheme](/doc/version-scheme/) documentation.
