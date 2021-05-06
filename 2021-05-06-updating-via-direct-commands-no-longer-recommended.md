---
layout: post
title: "Updating via direct commands no longer recommended"
categories:
 - security
 - announcements
author: Andrew David Wong
---

For many years now, Qubes users have been accustomed to updating their systems using commands like `qubes-dom0-update`, `dnf update`, and `apt update` (along with several variants). Recently, however, the Qubes team has made numerous security improvements to the way updates work in Qubes OS (see below for examples). In order to benefit from these improvements, it is important that users switch from using the old direct commands to using the **Qubes Update** tool or its command-line equivalents, which are documented in our newly-revised [Updating Qubes OS](/doc/updating-qubes-os/) page.

_**Note:** This change is only about **updating**. It's still fine to use direct commands for **installing** new packages._

## User action summary

 - **Do** keep updating your system frequently, including dom0, TemplateVMs, and StandaloneVMs (if you have any).
 - **Avoid** direct use of old commands like `qubes-dom0-update`, `dnf update`, and `apt update` (and their variants).
 - **Do** use the **Qubes Update** tool (or its `qubesctl` command-line equivalents), as documented in [Updating Qubes OS](/doc/updating-qubes-os/).

## Security explanation and examples

When you update using the **Qubes Update** tool (or its `qubesctl` command-line equivalents), you are updating via the [Salt management stack](/doc/salt/). Salt allows the Qubes developers to deliver security fixes that cannot be handled by normal package updates. For example, you can think of updating a Fedora TemplateVM with the **Qubes Update** tool as doing `dnf update` *plus* any available Salt configuration steps from the Qubes developers. In some cases, certain security bugs can only be fixed via these Salt configuration steps. They can't be handled by a package installed via `dnf` or another package manager alone.

There are four main examples that illustrate the importance of updating via Salt:

1. [QSB-046 / DSA-4371-1: APT update mechanism vulnerability](/news/2019/01/23/qsb-46/). Since the security flaw was in the APT update mechanism itself, the alternative patching method described in QSB-046 relied on using special Salt configuration steps to fix it. It was very important to fix APT updates *before* attempting to use APT to update normally.

2. [QSB-067: Multiple RPM vulnerabilities](/news/2021/03/19/qsb-067/). Demi M. Obenour discovered several security issues in the RPM package manager. As in the previous example, these were security flaws in the update mechanism itself. In this case, Salt was used to apply a mitigation that prevented privilege escalation as well as to strengthen authentication by forcing RPM to always verify package signatures. Users who updated exclusively via `dnf` would not have received these security benefits.

3. Qubes issue [#6275](https://github.com/QubesOS/qubes-issues/issues/6275). By default, `repo_gpgcheck` is absent from Qubes repository definitions in dom0. Users can manually add `repo_gpgcheck=1` and will gain a security benefit from doing so. This can cause problems due to bugs in DNF. However, the scripts used by the Qubes Update tool and its command-line equivalents are aware of the bugs in DNF and work around them.

4. In dom0's repository definition files, there are specific Tor onion URLs specified for retrieving packages from Tor onion services. At one point, these URLs changed. In order to update these URLs in everyone's repository definitions (and to check that they've been updated correctly), it was necessary to use Salt. Users who updated exclusively with `qubes-dom0-update` would not have received this fix.

As these examples show, the main reason to update via Salt rather than via direct commands is to benefit from security features that are only available via Salt updates. The time when this is most important is when a major new security issue requires fixes that can only be delivered via Salt. When that occurs we will, as always, issue a [Qubes Security Bulletin (QSB)](/security/bulletins/) that explicitly states how to update your system in order to receive the correct security fixes.

To be clear, this announcement is *not* saying that updating via direct commands is inherently any less safe than it was before. Rather, we are emphasizing that updating via Salt can be *more* secure in certain circumstances. For most users, this is a good enough reason to switch to updating via Salt all the time, since getting into the consistent habit of updating via Salt decreases the odds of accidentally missing out on any security benefits. However, some advanced users may still insist on using the old direct commands. We encourage those users to pay close attention to all new QSBs we issue and, in particular, to note when special security fixes require updating via Salt.
