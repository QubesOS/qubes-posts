---
layout: post
title: "Thank you, Joanna!"
date: 2018-10-25
categories: announcements
author: Marek Marczykowski-Górecki
---

The Qubes OS project was [founded by Joanna Rutkowska in 2009][qubes-founded].
I joined the project in its early days, before Qubes 1.0, and have been part of
the team under Joanna's leadership since then. Over the past nine years, the
system architecture has been enhanced multiple times, including major changes
like [HVM with stubdomain support][windows], the [Hypervisor Abstraction Layer
(HAL)][HAL], and finally, in Qubes 4.0, the [Admin API][admin-api] and [switch
to PVH][pvh] as the main VM type. The project has also matured a lot. We
started as a set of [a few][original-packages] [manually built][build] packages
installed [on top of Fedora 12][alpha-1-install]. Now, we have a [build
infrastructure][build-infra], documented [versioning scheme][version-scheme]
and [release schedules][release-schedules], [coding guidelines][coding-style],
and [automated tests][automated-tests]. The core part of Qubes has also been
rewritten a few times since its original release.  The project's success can be
measured by its growing community, including deployments like [SecureDrop] and
[Let's Encrypt].

Today [Joanna announced][joanna-post] that she is stepping down from the
project's leadership role and nominating me as her successor. I have been the
project's lead engineer for a few years now, and I'm honored to officially lead
the project as a whole. I plan to continue the direction in which Qubes OS has
been going, providing defenses [well ahead][netvm-tweet] of new attacks.

On behalf of the whole Qubes team, I'd like to thank Joanna for all of her
years of work on the project. Under her leadership, Qubes OS has accomplished a
lot, with only some of its many successes mentioned above. We look forward to
continuing to benefit from her expertise as an advisor. At the same time, we
wish her all the best in her new role on the Golem Project!

[qubes-founded]: https://blog.invisiblethings.org/2010/04/07/introducing-qubes-os.html
[HAL]: https://blog.invisiblethings.org/2013/03/21/introducing-qubes-odyssey-framework.html
[windows]: https://blog.invisiblethings.org/2012/12/14/qubes-2-beta-1-with-initial-windows.html
[admin-api]: https://blog.invisiblethings.org/2017/06/27/qubes-admin-api.html
[mgmt-stack]: https://www.qubes-os.org/news/2015/12/14/mgmt-stack/
[pvh]: https://www.qubes-os.org/news/2016/09/02/4-0-minimum-requirements-3-2-extended-support/
[alpha-1-install]: https://github.com/QubesOS/qubes-doc/blob/d6639edf47a7b85e54cd470380de25e1b7403407/InstallationGuide.md
[build]: https://groups.google.com/d/msg/qubes-devel/cQ9yVxPMfoo/CTIXml3B_NcJ
[original-packages]: https://github.com/QubesOS/qubes-doc/blob/6ac51fb134093168ec3900c9bed22c3a86bcd021/SourceCode.md
[build-infra]: https://github.com/QubesOS/qubes-infrastructure/blob/master/README.md#detailed-description-of-the-infrastructure
[release-schedules]: https://www.qubes-os.org/doc/releases/schedules/
[coding-style]: https://www.qubes-os.org/doc/coding-style/
[automated-tests]: https://www.qubes-os.org/doc/automated-tests/
[version-scheme]: https://www.qubes-os.org/doc/version-scheme/
[joanna-post]: /news/2018/10/25/the-next-chapter/
[netvm-tweet]: https://twitter.com/rootkovska/status/530416582426902528
[SecureDrop]: https://securedrop.org/news/road-towards-integrated-securedrop-workstation/
[Let's Encrypt]: https://twitter.com/RMLLsec16/status/749982515948027904
