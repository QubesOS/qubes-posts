---
layout: post
title: "Estimating Qubes OS user base"
date: 2016-01-14
author: Wojtek Porczyk
categories:
  - announcements
---

Qubes OS has been developed for five years now and we know there is a growing
number of people who use it daily. There are many of them writing on our
mailing lists.  But we never really had a quantitative metric of how many
intallations there are and how many people exactly are using it. We need a more
specific metric for several purposes: we need to know which versions are used
in order to be able to use our constrained manpower efiiciently. The statistics
are also needed when reaching out to our sources of funding.

There are problems with gathering statistics. First and foremost, there are
privacy implications to this kind of research and we are well aware that too
invasive gathering or even publishing too granular metrics would violate the
privacy rights of our users. At ITL, we believe that users' security depends
on their privacy and our conscience would not let us "monetise" (that is,
exploit) our userbase. We would never exchange it even for much needed project
funding.

One commonly found solution is to provide some kind of "user choice", through
which users could agree (or not) to being counted. This however would not
provide reliable data, since we expect people would generally not enable that
option. There are also many other problems with this approach: I personally
encountered one project which, when such phoning home was enabled, would allow
remote arbitrary code execution by the project maintainers and anyone who can
issue a valid TLS certificate for their domain.

Therefore, we decided that we would analyse logs of our update server, which
currently serves the YUM repositories. We count the number of unique addresses
which connect to the server and download index files of our repositories for
different Qubes OS releases to see which release is used most. We also count
them without any differentiation in releases to estimate number of actual users.

We have already some observations. The number of people using R2 is shrinking
steadily and there are many more people who use R3.0 stable release. Some of
them have already switched to R3.1, which currently is the release candidate.
Also, there are IP addresses which download more than one release.

There is a non-negligible number of people who download their updates over Tor.
This distorts the statistics, because we don't know how many people are behind
the IP addresses of exit nodes. In fact, in the R3.1 release we support setting
Whonix Gateway as updatevm by the preconfiguration subsystem, so we expect that
number to grow and further skew the statistics.

The statistics are available at
[https://www.qubes-os.org/counter/](/counter/):

![Estimated Qubes OS userbase graph](https://tools.qubes-os.org/counter/stats.png)

What is currently missing:

- Installation media ISO images. Those are primarily served by
  mirror.kernel.org and Linux Foundation doesn't keep statistics on particular
  files.
- Templates. We don't really need to distinguish between people who install
  Debian or Whonix templates, so we don't keep that metric.

UPDATED 14.01.2016 15:17 UTC+1: editing, spelling+grammar
