---
layout: post
title: "Qubes OS 4.1 has reached end-of-life; extended security support continues until 2024-07-31"
categories:
 - announcements
 - releases
redirect_from: /news/2024/06/18/qubes-os-4-1-has-reached-end-of-life/
---

As [previously announced](/news/2024/03/26/qubes-os-4-1-reaches-eol-on-2024-06-18/), the Qubes OS 4.1 release series has officially reached end-of-life (EOL) as of today, 2024-06-18. However, Qubes OS 4.1 [will continue to receive extended security support until 2024-07-31](/news/2024/05/10/qubes-os-4-1-to-receive-extended-support-until-2024-07-31/). We recommend that all remaining Qubes 4.1 users [upgrade to Qubes 4.2](/doc/upgrade/4.2/) at this time.

## Recommended actions

If you're already using Qubes 4.2, then you don't have to do anything. This announcement doesn't affect you.

If you're still using Qubes 4.1, then you should upgrade to Qubes 4.2 at your earliest convenience but no later than 2024-07-31. There are two ways to do this:

1. Perform a [clean reinstallation](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_2.html#clean-installation) using the latest stable [Qubes OS 4.2.1 ISO](/downloads/).
2. Perform an [in-place upgrade to Qubes 4.2](https://doc.qubes-os.org/en/latest/user/downloading-installing-upgrading/upgrade/4_2.html#in-place-upgrade).

Both of these options are covered in further detail in the [Qubes 4.1 to 4.2 upgrade guide](/doc/upgrade/4.2/). In either case, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand. If you need help, please consult our [help and support](/support/) page.

## What does end-of-life (EOL) mean?

When a Qubes OS release reaches end-of-life (EOL), it is no longer supported. In the case of Qubes 4.1, this means that enhancements will no longer be added and non-security bugs will no longer be fixed. Security bugs will still be fixed until 2024-07-31, since Qubes 4.1 has [extended security support](/news/2024/05/10/qubes-os-4-1-to-receive-extended-support-until-2024-07-31/) until then. After 2024-07-31, Qubes 4.1 will no longer have security support either, which means that it will not be supported at all.

## What is extended security support?

Extended security support means that the [Qubes security team](https://doc.qubes-os.org/en/latest/project-security/security.html#qubes-security-team) will continue to publish [Qubes security bulletins (QSBs)](/security/qsb/) and release security updates for security vulnerabilities affecting Qubes 4.1, as it deems appropriate, until 2024-07-31. Extended security support does *not* cover regular bug fixes, improvements, or the implementation of new features.

## What about patch releases?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. When a major or minor release reaches EOL, all of its patch releases also reach EOL. For example, in this case, when we say that "Qubes 4.1" (without specifying a `<patch>` number) has reached EOL, we're specifying a particular minor release inclusive of all patch releases within it. This means that Qubes 4.1.0, 4.1.1, and 4.1.2 have all reached EOL, since they are all patch releases of the same minor release.

## How are EOL dates determined?

According to our [support policy](/doc/supported-releases/), stable Qubes OS releases are supported for **six months** after each subsequent [major or minor release](/doc/version-scheme/). This means that the EOL date for Qubes 4.1 was set at the time Qubes 4.2 was released by adding six months to the Qubes 4.2 release date. Qubes 4.2.0 was [released on 2023-12-18](/news/2023/12/18/qubes-os-4-2-0-has-been-released/). Adding six months to this date gives us 2024-06-18, which is Qubes 4.1's EOL date.
