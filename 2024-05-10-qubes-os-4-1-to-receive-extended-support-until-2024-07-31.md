---
layout: post
title: "Qubes OS 4.1 to receive extended security support until 2024-07-31"
categories:
- announcements
- releases
author: FPF and the Qubes team
---

Qubes OS 4.1 will reach official end-of-life (EOL) on 2024-06-18. After this date, Qubes OS 4.1 will continue to receive extended security support until 2024-07-31. This security support extension is sponsored by [Freedom of the Press Foundation (FPF)](https://freedom.press/) in support of the [SecureDrop](https://securedrop.org/) project.

## What's happening?

According to the Qubes OS Project's [release support policy](/doc/supported-releases/), Qubes OS releases are supported for six months after each subsequent [major or minor release](/doc/version-scheme/). This means that Qubes 4.1 will reach EOL six months after Qubes 4.2 was released. Since Qubes 4.2 was [released](/news/2023/12/18/qubes-os-4-2-0-has-been-released/) on 2023-12-18, Qubes 4.1 is [scheduled](/news/2024/03/26/qubes-os-4-1-reaches-eol-on-2024-06-18/) to reach EOL six months later on 2024-06-18.

[SecureDrop](https://securedrop.org/) currently relies on Qubes 4.1 for the [SecureDrop Workstation](https://workstation.securedrop.org/). To allow for additional time to ensure full compatibility of the SecureDrop Workstation with Qubes 4.2, and to help existing users migrate, FPF has offered to sponsor an extension of post-EOL Qubes 4.1 security support until 2024-07-31, and the Qubes OS Project has agreed.

## What does extended security support cover?

The [Qubes security team](https://doc.qubes-os.org/en/latest/project-security/security.html#qubes-security-team) will continue to publish [Qubes security bulletins (QSBs)](/security/qsb/) and release security updates for security vulnerabilities affecting Qubes 4.1, as it deems appropriate, until 2024-07-31. Extended security support does *not* cover regular bug fixes, improvements, or the implementation of new features.

In short, if you currently have a Qubes 4.1 installation that serves your needs, you may safely continue to use it until 2024-07-31, but we strongly recommend [upgrading to Qubes 4.2](/doc/upgrade/4.2/) by that date.

## What's involved in extending security support for Qubes 4.1?

Extending support for a Qubes release is more challenging than it might seem on the surface. Here are some of the main considerations involved:

1. **Xen support:** Official support for Xen 4.14 has already ended, which means that backporting Xen-related security fixes will require more work than usual.

2. **Template support:** Qubes 4.1 supports Debian 11, which has quite old software. This is known to cause problems and to require workarounds (e.g., with `salt` and `app-u2f`). There will be no Fedora 40 template for Qubes 4.1, but that's okay since Fedora 39 doesn't reach EOL until November.

3. **Other dom0 software:** Qubes 4.1's dom0 is based on Fedora 32, which is now quite old. If we end up having to backport any fixes here (e.g., due to an RPM or GPG bug), it may be quite complicated.

4. **Whonix support:** Any extension of the support period for a Qubes release must also take into consideration Whonix support. Previously, [Whonix 16 reached EOL](/news/2023/12/22/whonix-16-approaching-eol/) even though Qubes 4.1 has not yet reached EOL. Whonix 17 did not support Qubes 4.1 at the time, which meant users on Qubes 4.1 were at risk of being left without any supported way to continue using Whonix. The Whonix and Qubes teams successfully bridged this gap by [making Whonix 17 available on Qubes 4.1](/news/2024/02/05/whonix-17-templates-available-for-qubes-os-4-1/). Now, Qubes 4.1 will receive extended security support, which will require a commensurate extension of security support for Whonix 17 on Qubes 4.1. FPF and the Whonix Project have arranged for the required Whonix 17 support extension to be included with the Qubes 4.1 extension, so Whonix 17 security support on Qubes 4.1 will continue until 2024-07-31.
