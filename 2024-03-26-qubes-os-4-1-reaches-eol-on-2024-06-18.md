---
layout: post
title: "Qubes OS 4.1 reaches EOL on 2024-06-18"
categories:
  - announcements
  - releases
---

Qubes OS 4.1 is scheduled to reach [end-of-life (EOL)](#what-does-end-of-life-eol-mean) on 2024-06-18, approximately three months from the date of this announcement.

## Recommended actions

If you're already using Qubes 4.2, then you don't have to do anything. This announcement doesn't affect you.

If you're still using Qubes 4.1, then now is the perfect opportunity to upgrade, since a brand new [Qubes OS 4.2.1 ISO was just released today](/news/2024/03/26/qubes-os-4-2-1-has-been-released/)! (This is also the best way to get started with Qubes if you don't have it installed yet.)

If you'd prefer not to reinstall, you can instead perform an [in-place upgrade from Qubes 4.1 to 4.2](/doc/upgrade/4.2/#in-place-upgrade).

Whichever option you choose, we strongly recommend [making a full backup](/doc/how-to-back-up-restore-and-migrate/) beforehand and ensuring you're on Qubes 4.2 by 2024-06-18.

## What does end-of-life (EOL) mean?

When a Qubes OS release reaches end-of-life (EOL), it is no longer supported. This means that bugs discovered in that release will no longer be fixed, and enhancements will no longer be added. Most importantly, releases that have reached EOL no longer receive security updates, which is why it's critically important to upgrade to a supported release.

## What about patch releases?

The Qubes OS Project uses the [semantic versioning](https://semver.org/) standard. Version numbers are written as `[major].[minor].[patch]`. When a major or minor release reaches EOL, all of its patch releases also reach EOL. For example, in this case, when we say that "Qubes 4.1" (without specifying a `<patch>` number) is approaching EOL, we're specifying a particular minor release, inclusive of all patch releases within it. This means that Qubes 4.1.0, 4.1.1, and 4.1.2 will all reach EOL at the same time (on 2024-06-18), since they are all just patch releases of the same minor release.

## How are EOL dates determined?

According to our [support policy](/doc/supported-releases/), stable Qubes OS releases are supported for six months after each subsequent [major or minor release](/doc/version-scheme/). This means that Qubes 4.1 reaches EOL six months after Qubes 4.2 was released. Since Qubes 4.2.0 was [released on 2023-12-18](/news/2023/12/18/qubes-os-4-2-0-has-been-released/), Qubes 4.1's EOL date is six months later, on 2024-06-18.
