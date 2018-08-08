---
layout: post
title: "Whonix 14 has been released"
date: 2018-08-07
categories: announcements
author: Patrick Schleizer
---

After more than two years of development, the Whonix Project is proud
to announce the release of Whonix 14.

Whonix 14 is based on the Debian stretch (Debian 9) distribution which
was released in June 2017. This means users have access to many new
software packages in concert with existing packages such as a modern
branch of GNuPG, and more. [[1]][[2]][[3]]

Major Changes and New Features
------------------------------

Whonix 14 contains extensive security and usability improvements, new
features and bug fixes. For a detailed description of these and other
changes, please refer to the official release notes. [[4]]

* Rebased Whonix on **Debian stretch** (Debian 9).
* Whonix 14 is **64-bit** (amd64) only - 32-bit (i386) images will no
longer be built and made available for download. [5]
* The new **Anon Connection Wizard** [[6]] feature in Whonix simplifies
connections to the Tor network via a Tor bridge and/or a proxy.
* The Tor pluggable transport **meek_lite** [[7]] is now supported,
making it much easier to connect to the Tor network in heavily
censored areas, like China. [[8]]
* **Onioncircuits** are installed by default in Whonix. [[9]]
* Tails' **onion-grater** program has been implemented to enable
**OnionShare, Ricochet and Zeronet** compatibility with Whonix. [[10]]
* **Onion sources** are now preferred for Whonix updates/upgrades for
greater security.
* Reduced the size of the default, binary Whonix images by
approximately **35 per cent** using zerofree. [[11]] [12]
* **Updated Tor** to version 3.3.7 (stable) release to enable full v3
onion functionality for both hosting of onion services and access to
v3 onion addresses in Tor Browser.
* Created the **grub-live package** [[13]] which can run Whonix as a
**live system** on non-Qubes-Whonix platforms. [14]
* Corrected and hardened various **AppArmor profiles** to ensure the
correct functioning of Tor Browser, obfsproxy and other applications.


Known Issues
------------

* Desktop shortcuts are no longer available in non-Qubes-Whonix.
* OnionShare is not installed by default in Whonix 14 as it is not in
the stretch repository. [[15]] It can still be manually installed by
following the Whonix wiki instructions [[16]] or building it from source
code. [[17]]
* Enabling seccomp (Sandbox 1) in `/usr/local/etc/torrc.d/50_user.conf`
causes the Tor process to crash if a Tor version lower than 0.3.3 is
used. [[18]] [[19]]


While there may be other issues that exist in this declared stable
release, every effort has been made to address major known problems.

Please report any other issues to us in the forums, after first
searching for whether it is already known.

  <https://www.whonix.org/wiki/Known_Issues>

Download Whonix 14
------------------

Whonix is cross-platform and can be installed on the Windows, macOS,
Linux or Qubes operating systems. Choose your operating system from
the link below and follow the instructions to install it.

  <https://www.whonix.org/download/>

Upgrade to Whonix 14
--------------------

Current Whonix users (or those with 32-bit hardware) who would prefer
to upgrade their existing Whonix 13 platform should follow the upgrade
instructions below.

  <https://whonix.org/wiki/Upgrading_Whonix_13_to_Whonix_14>

What’s Next?
------------

Work on Whonix 15 is ongoing and interested users can refer to the
roadmap to see where Whonix is heading. [[20]]

Developer priorities are currently focused on easing the transition to
the next Debian release due in 2019 (“buster”; Debian 10) and
squashing existing bugs, rather than implementing new features.

We need your help and there are various ways to contribute to Whonix -
donating or investing your time will help the project immensely. Come
and talk with us! [[21]]

Notes
-----

[5] Whonix 13 users with 32-bit systems can however upgrade their
platform by following the available wiki instructions, rather than
download new Whonix-WS and Whonix-GW images.

[12] VirtualBox .ova and libvirt qcow2 raw images. The Whonix-Gateway
is reduced from 1.7 GB to 1.1 GB, while the Whonix-Workstation is
reduced from 2 GB to 1.3 GB.

[14] grub-live is optional and requires the user to first enable it
manually.

----------

*This post has been formatted for presentation on the Qubes website from the [original mailing list announcement](https://groups.google.com/forum/#!topic/qubes-users/5mBXxQ_LSvg).*

[1]: https://www.debian.org/News/2017/20170617
[2]: https://www.debian.org/releases/stable/amd64/release-notes/
[3]: https://www.debian.org/releases/stable/i386/release-notes/
[4]: https://whonix.org/wiki/Whonix_Release_Notes#Whonix_14
[6]: https://whonix.org/wiki/Anon_Connection_Wizard
[7]: https://www.whonix.org/blog/meek_lite-whonix-14
[8]: https://github.com/Yawning/obfs4/commit/611205be681322883a4d73dd00fcb13c4352fe53
[9]: https://packages.debian.org/stretch/onioncircuits
[10]: https://phabricator.whonix.org/T657
[11]: https://phabricator.whonix.org/T790
[13]: https://whonix.org/wiki/Whonix_Live
[15]: https://packages.debian.org/search?searchon=names&keywords=onionshare
[16]: https://whonix.org/wiki/Onionshare
[17]: https://github.com/micahflee/onionshare/blob/master/BUILD.md#gnulinux
[18]: https://trac.torproject.org/projects/tor/ticket/22605
[19]: https://packages.debian.org/stretch/tor
[20]: https://phabricator.whonix.org/maniphest/query/open/
[21]: https://forums.whonix.org

