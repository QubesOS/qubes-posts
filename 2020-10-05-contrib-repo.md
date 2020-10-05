---
layout: post
title: Qubes contrib packages repository available
categories: announcements
author: Marek Marczykowski-Górecki
---

We are happy to announce the availability of contributed packages repository. This is the place where our community can publish Qubes OS related packages, additions and various customizations. On the other hand we provide the infrastructure and review process to make them available easily and safely in standard Qubes installation.

The infrastructure was built by Frédéric Pierret, based on similar setup for building official packages. This means the same build security measurements are taken, including keeping the signing keys separate in a dedicated VM, downloading packages over Tor, publishing build logs in a non-spoofable way and few more. Frédéric is also maintainer of QubesOS-contrib repositories.
The source code repositories of the packages and also infrastructure-related parts are hosted on github in a separate [QubesOS-contrib] organization.

To contribute your package into the repository, follow a process described at [package contributions]. You will find there few helpful tips, including a [skeleton repository] with example rpm packaging and qubes-builder integration.
Since the repository is running already for some time, some submitted packages are already available there. To name a few:

 - [qubes-tunnel]
 - [qvm-screenshot-tool]
 - [qmenu]

You can find the full list at [QubesOS-contrib].

If you want to install package from this repository, first you need to enable it in your system (dom0 and/or templates). This can be done by installing `qubes-repo-contrib` package - it includes appropriate repository definition and keys necessary to verify the packages.

To install it in dom0, use `qubes-dom0-update`:

    sudo qubes-dom0-update qubes-repo-contrib

To install it in a Debian-based template, use `apt`:

    sudo apt update && sudo apt install qubes-repo-contrib

To install it in a Fedora-based template, use `dnf`:

    sudo dnf install qubes-repo-contrib


[QubesOS-contrib]: https://github.com/QubesOS-contrib/
[package contributions]: /doc/package-contributions/
[skeleton repository]: https://github.com/QubesOS-contrib/qubes-skeleton/
[qubes-tunnel]: https://github.com/QubesOS-contrib/qubes-tunnel
[qvm-screenshot-tool]: https://github.com/QubesOS-contrib/qvm-screenshot-tool
[qmenu]: https://github.com/QubesOS-contrib/qmenu
