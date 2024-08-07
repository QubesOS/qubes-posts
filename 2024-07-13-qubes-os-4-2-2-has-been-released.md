---
layout: post
title: "Qubes OS 4.2.2 has been released!"
categories: releases
download_url: /downloads/
---

We're pleased to announce the stable release of Qubes OS 4.2.2! This patch release aims to consolidate all the security patches, bug fixes, and other updates that have occurred since the previous stable release. Our goal is to provide a secure and convenient way for users to install (or reinstall) the latest stable Qubes release with an up-to-date ISO. The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## What's new in Qubes 4.2.2?

- All security updates to date
- All bug fixes to date
- Included Fedora template upgraded from Fedora 39 to 40
- Fixed [#8332: File-copy qrexec service is overly restrictive](https://github.com/QubesOS/qubes-issues/issues/8332) (see below)

For more information about the changes included in this version, see the [Qubes OS 4.2 release notes](/doc/releases/4.2/release-notes/) and the [full list of issues completed since the previous stable release](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue+is%3Aclosed+reason%3Acompleted+closed%3A2024-03-26..2024-06-23+-label%3A%22R%3A+cannot+reproduce%22+-label%3A%22R%3A+declined%22+-label%3A%22R%3A+duplicate%22+-label%3A%22R%3A+not+applicable%22+-label%3A%22R%3A+self-closed%22+-label%3A%22R%3A+upstream+issue%22).

### Copying and moving files between qubes is less restrictive

Qubes 4.2.2 includes a fix for [#8332: File-copy qrexec service is overly restrictive](https://github.com/QubesOS/qubes-issues/issues/8332). As explained in the issue comments, we introduced a change in Qubes 4.2.0 that caused inter-qube file-copy/move actions to reject filenames containing, e.g., non-Latin characters and certain symbols. The rationale for this change was to mitigate the security risks associated with unusual unicode characters and invalid encoding in filenames, which some software might handle in an unsafe manner and which might cause confusion for users. Such a change represents a trade-off between security and usability.

After the change went live, we received several user reports indicating more severe usability problems than we had anticipated. Moreover, these problems were prompting users to resort to dangerous workarounds (such as packing files into an archive format prior to copying) that carry far more risk than the original risk posed by the unrestricted filenames. In addition, we realized that this was a backward-incompatible change that should not have been introduced in a minor release in the first place.

Therefore, we have decided, for the time being, to restore the original (pre-4.2) behavior by introducing a new `allow-all-names` argument for the `qubes.Filecopy` service. By default, `qvm-copy` and similar tools will use this less restrictive service (`qubes.Filecopy +allow-all-names`) whenever they detect any files that would be have been blocked by the more restrictive service (`qubes.Filecopy +`). If no such files are detected, they will use the more restrictive service.

Users who wish to opt for the more restrictive 4.2.0 and 4.2.1 behavior can do so by modifying their RPC policy rules. To switch a single rule to the more restrictive behavior, change `*` in the argument column to `+` (i.e., change "any argument" to "only empty"). To use the more restrictive behavior globally, add the following "deny" rule before all other relevant rules:

```
qubes.Filecopy    +allow-all-names    @anyvm    @anyvm    deny
```

For more information, see [RPC policies](/doc/rpc-policy/) and [Qube configuration interface](/doc/vm-interface/#qubes-rpc).

## How to get Qubes 4.2.2

You have a few different options, depending on your situation:

- If you'd like to install Qubes OS for the first time or perform a clean reinstallation on an existing system, there's never been a better time to do so! Simply [download](/downloads/) the Qubes 4.2.2 ISO and follow our [installation guide](/doc/installation-guide/).

- If you're currently on Qubes 4.1, learn [how to upgrade to Qubes 4.2](/doc/upgrade/4.2/).

- If you're currently on Qubes 4.2 (including 4.2.0, 4.2.1, and 4.2.2-rc1), [update normally](/doc/how-to-update/) (which includes [upgrading any EOL templates](/doc/how-to-update/#upgrading-to-avoid-eol) you might have) in order to make your system essentially equivalent to the stable Qubes 4.2.2 release. No reinstallation or other special action is required.

In all cases, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand.

## Reminder: new signing key for Qubes 4.2

As a reminder for those upgrading from Qubes 4.1 and earlier, we published the following special announcement in [Qubes Canary 032](/news/2022/09/14/canary-032/) on 2022-09-14:

> We plan to create a new Release Signing Key (RSK) for Qubes OS 4.2. Normally, we have only one RSK for each major release. However, for the 4.2 release, we will be using Qubes Builder version 2, which is a complete rewrite of the Qubes Builder. Out of an abundance of caution, we would like to isolate the build processes of the current stable 4.1 release and the upcoming 4.2 release from each other at the cryptographic level in order to minimize the risk of a vulnerability in one affecting the other. We are including this notice as a canary special announcement since introducing a new RSK for a minor release is an exception to our usual RSK management policy.

As always, we encourage you to [authenticate](/security/pack/#how-to-obtain-and-authenticate) this canary by [verifying its PGP signatures](/security/verifying-signatures/). Specific instructions are also included in the [canary announcement](/news/2022/09/14/canary-032/).

As with all Qubes signing keys, we also encourage you to [authenticate](/security/verifying-signatures/#how-to-import-and-authenticate-release-signing-keys) the Qubes OS Release 4.2 Signing Key, which is available in the [Qubes Security Pack (qubes-secpack)](/security/pack/) as well as on the [downloads](/downloads/) page.

## What is a patch release?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `<major>.<minor>.<patch>`. Hence, we refer to releases that increment the third number as "patch releases." A patch release does not designate a separate, new major or minor release of Qubes OS. Rather, it designates its respective major or minor release (in this case, 4.2) inclusive of all updates up to a certain point. (See [supported releases](/doc/supported-releases/) for a comprehensive list of major and minor releases.) Installing the initial Qubes 4.2.0 release and fully [updating](/doc/how-to-update/) it results in essentially the same system as installing Qubes 4.2.2. You can learn more about how Qubes release versioning works in the [version scheme](/doc/version-scheme/) documentation.
