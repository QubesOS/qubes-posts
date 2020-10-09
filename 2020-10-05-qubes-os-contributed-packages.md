---
layout: post
title: Qubes OS contributed packages are now available
categories: announcements
author: Marek Marczykowski-Górecki
---

We are happy to announce the availability of Qubes OS contributed packages under the [QubesOS-contrib] GitHub Project. This is a place where our community can [contribute Qubes OS related packages, additions and various customizations][package-contrib]. Meanwhile, we provide the infrastructure and [review process] necessary to make them available easily and safely to users within standard Qubes installations.

[Frédéric Pierret] built the infrastructure based on a similar setup for building official packages. This means that it features the same [Qubes build security] measures, including keeping the signing keys separate in a dedicated VM, downloading packages over Tor, publishing build logs in a non-spoofable way and more. Frédéric is also the maintainer of [QubesOS-contrib].
The source code repositories of the packages and infrastructure-related parts are also hosted under [QubesOS-contrib].

To contribute a package, follow the process described at [package contributions]. You will find a few helpful tips there, including a [skeleton repository] with example RPM packaging and [Qubes Builder] integration.
Since the project has been running for some time already, there are already some submitted packages available there. To name a few:

 - [qubes-tunnel]
 - [qvm-screenshot-tool]
 - [qmenu]

You can find the full list at [QubesOS-contrib].

If you want to install one of these packages, first you need to enable the repository in your system (dom0 and/or templates). This can be done by installing the `qubes-repo-contrib` package. This package includes the repository definition and keys necessary to download, verify, and install [QubesOS-contrib] packages.

In dom0, use `qubes-dom0-update`:

    sudo qubes-dom0-update qubes-repo-contrib

In a Fedora-based template, use `dnf`:

    sudo dnf install qubes-repo-contrib

In a Debian-based template, use `apt`:

    sudo apt update && sudo apt install qubes-repo-contrib


[QubesOS-contrib]: https://github.com/QubesOS-contrib/
[package-contrib]: https://www.qubes-os.org/doc/package-contributions/
[review process]: https://www.qubes-os.org/doc/package-contributions/#review-procedure
[Frédéric Pierret]: https://www.qubes-os.org/team/#fr%C3%A9d%C3%A9ric-pierret
[Qubes build security]: https://www.qubes-os.org/news/2016/05/30/build-security/
[package contributions]: https://www.qubes-os.org/doc/package-contributions/
[skeleton repository]: https://github.com/QubesOS-contrib/qubes-skeleton/
[Qubes Builder]: https://www.qubes-os.org/doc/qubes-builder/
[qubes-tunnel]: https://github.com/QubesOS-contrib/qubes-tunnel
[qvm-screenshot-tool]: https://github.com/QubesOS-contrib/qubes-qvm-screenshot-tool
[qmenu]: https://github.com/QubesOS-contrib/qmenu
