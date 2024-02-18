---
layout: post
title: "Qubes OS 4.2.0 has been released!"
categories: releases
download_url: /downloads/
image: /attachment/site/4-2_global-config_1.png
---

Qubes OS 4.2.0 brings a host of new features, major improvements, and numerous bug fixes. The ISO and associated [verification files](/security/verifying-signatures/) are available on the [downloads](/downloads/) page.

## What's new in Qubes OS 4.2.0?

- Dom0 upgraded to Fedora 37 ([#6982](https://github.com/QubesOS/qubes-issues/issues/6982))
- Xen upgraded to version 4.17
- Default Debian template upgraded to Debian 12
- Default Fedora and Debian templates use Xfce instead of GNOME ([#7784](https://github.com/QubesOS/qubes-issues/issues/7784))
- SELinux support in Fedora templates ([#4239](https://github.com/QubesOS/qubes-issues/issues/4239))
- Several GUI applications rewritten (screenshots below), including:
  - Applications Menu (also available as preview in R4.1) ([#6665](https://github.com/QubesOS/qubes-issues/issues/6665)), ([#5677](https://github.com/QubesOS/qubes-issues/issues/5677))
  - Qubes Global Settings ([#6898](https://github.com/QubesOS/qubes-issues/issues/6898))
  - Create New Qube
  - Qubes Update ([#7443](https://github.com/QubesOS/qubes-issues/issues/7443))
- Unified `grub.cfg` location for both UEFI and legacy boot ([#7985](https://github.com/QubesOS/qubes-issues/issues/7985))
- PipeWire support ([#6358](https://github.com/QubesOS/qubes-issues/issues/6358))
- fwupd integration for firmware updates ([#4855](https://github.com/QubesOS/qubes-issues/issues/4855))
- Optional automatic clipboard clearing ([#3415](https://github.com/QubesOS/qubes-issues/issues/3415))
- Official packages built using Qubes Builder v2 ([#6486](https://github.com/QubesOS/qubes-issues/issues/6486))
- Split GPG management in Qubes Global Settings
- Qrexec services use new qrexec policy format by default (but old format is still supported) ([#8000](https://github.com/QubesOS/qubes-issues/issues/8000))
- Improved keyboard layout switching

For further details, see the [Qubes 4.2 release notes](/doc/releases/4.2/release-notes/) and the [full list of issues completed for Qubes 4.2](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue+is%3Aclosed+reason%3Acompleted+milestone%3A%22Release+4.2%22+-label%3A%22R%3A+cannot+reproduce%22+-label%3A%22R%3A+declined%22+-label%3A%22R%3A+duplicate%22+-label%3A%22R%3A+not+applicable%22+-label%3A%22R%3A+self-closed%22+-label%3A%22R%3A+upstream+issue%22+). Below are some screenshots of the new and improved Qubes GUI tools.

The new Qubes OS Update tool:

[![Screenshot of the Qubes OS Update tool](/attachment/site/4-2_update.png)](/attachment/site/4-2_update.png)

The new Qubes OS Global Config tool:

[![Screenshot of the Qubes OS Global Config tool](/attachment/site/4-2_global-config_1.png)](/attachment/site/4-2_global-config_1.png)
[![Screenshot of the Qubes OS Global Config tool](/attachment/site/4-2_global-config_2.png)](/attachment/site/4-2_global-config_2.png)

The new Qubes OS Policy Editor tool:

[![Screenshot of the Qubes OS Policy Editor tool](/attachment/site/4-2_policy-editor.png)](/attachment/site/4-2_policy-editor.png)


## Known issues in Qubes OS 4.2.0

- DomU firewalls have completely switched to nftables. Users should add their custom rules to the `custom-input` and `custom-forward` chains. (For more information, see issues [#5031](https://github.com/QubesOS/qubes-issues/issues/5031) and [#6062](https://github.com/QubesOS/qubes-issues/issues/6062).)

- Templates restored in 4.2 from a pre-4.2 backup continue to target their original Qubes OS release repos. If you are using fresh templates on a clean 4.2 installation, or if you performed an [in-place upgrade from 4.1 to 4.2](/doc/upgrade/4.2/#in-place-upgrade), then this does not affect you. (For more information, see issue [#8701](https://github.com/QubesOS/qubes-issues/issues/8701).)

Also see the [full list of open bug reports affecting Qubes 4.2](https://github.com/QubesOS/qubes-issues/issues?q=is%3Aissue+label%3Aaffects-4.2+label%3A%22T%3A+bug%22+is%3Aopen).

We strongly recommend [updating Qubes OS](/doc/how-to-update/) immediately after installation in order to apply all available bug fixes.

## How to get Qubes OS 4.2.0

- If you don't have Qubes OS installed, or if you're currently on Qubes 4.0 or earlier, follow the [installation guide](/doc/installation-guide/).
- If you're currently on Qubes 4.1, learn [how to upgrade to Qubes 4.2](/doc/upgrade/4.2/).
- If you're currently on a Qubes 4.2 release candidate (RC), [update normally](/doc/how-to-update/).

In all cases, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand.

## Reminder: new release signing key for Qubes 4.2

As a reminder, we published the following special announcement in [Qubes Canary 032](/news/2022/09/14/canary-032/) on 2022-09-14:

> We plan to create a new Release Signing Key (RSK) for Qubes OS 4.2. Normally, we have only one RSK for each major release. However, for the 4.2 release, we will be using Qubes Builder version 2, which is a complete rewrite of the Qubes Builder. Out of an abundance of caution, we would like to isolate the build processes of the current stable 4.1 release and the upcoming 4.2 release from each other at the cryptographic level in order to minimize the risk of a vulnerability in one affecting the other. We are including this notice as a canary special announcement since introducing a new RSK for a minor release is an exception to our usual RSK management policy.

As always, we encourage you to [authenticate](/security/pack/#how-to-obtain-and-authenticate) this canary by [verifying its PGP signatures](/security/verifying-signatures/). Specific instructions are also included in the [canary announcement](/news/2022/09/14/canary-032/).

As with all Qubes signing keys, we also encourage you to [authenticate](/security/verifying-signatures/#how-to-import-and-authenticate-release-signing-keys) the new Qubes OS Release 4.2 Signing Key, which is available in the [Qubes Security Pack (qubes-secpack)](/security/pack/) as well as on the [downloads](/downloads/) page under the Qubes OS 4.2.0-rc5 ISO.

## Support for older releases

In accordance with our [release support policy](/doc/supported-releases/#qubes-os), Qubes 4.1 will remain supported for six months after the release of Qubes 4.2, until 2024-06-18. After that, Qubes 4.1 will no longer receive security updates or bug fixes.

[Whonix templates](https://www.whonix.org/wiki/Qubes) are created and supported by our partner, the [Whonix Project](https://www.whonix.org/). The Whonix Project has set its own support policy for Whonix templates in Qubes. For more information, see [Qubes-Whonix version support policy](https://www.whonix.org/wiki/About#Qubes_Hosts).

## Thank you to our partners, donors, contributors, and testers!

This release would not be possible without generous support from our [partners](/partners/) and [donors](/donate/), as well as [contributions](/doc/contributing/) from our active community members, especially [bug reports](/doc/issue-tracking/) from our [testers](/doc/testing/). We are eternally grateful to our excellent community for making the Qubes OS Project a great example of open-source collaboration.
