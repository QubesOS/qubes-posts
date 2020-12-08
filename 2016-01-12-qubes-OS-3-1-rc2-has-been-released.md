---
layout: post
title: "Qubes OS 3.1 rc2 has been released!"
date: 2016-01-12
categories:
  - releases
download_url: /downloads/
author: Marek Marczykowski-Górecki
---
Today we're releasing the second release candidate of Qubes 3.1, with many bug
fixed over the [previous release candidate][qubes-31-rc1-announced], 
including the ability to install with a username different than `user`.
All critical bugs we know of are fixed, there are some other 
non-critical bugs to iron out prior to the final release of 3.1. 

Read the [release notes][release-notes] for more details, installation and update
instructions. The installation image can be downloaded from [here][download].

We would like to thanks all the users testing release candidates and all the
bug reports! This allows us to make the final release as stable as possible.

The Qubes Live edition is still not finished unfortunately and not yet part of this
release candidate. Our main focus is on standard edition and the Qubes 
development team currently doesn't have enough capacity to finish Live 
edition in time. Hopefully this will change with the final release of 
3.1.

While most of our team is working on finishing Qubes 3.1, we (most notably
Wojtek Porczyk) are also preparing the next major release - Qubes 4.0. Major
feature of this version will be mostly rewritten VM management code - the code
behind all the qvm tools and Qubes Manager. The new code is much better
organised, much more extensible. This, together with management stack
introduced in Qubes 3.1 will be a foundation for many flexible and 
powerful use-cases and workflows.

The first technology preview of Qubes 4.0 has already [been released][qubes-40-tp-release], 
however it isn't ready for normal use. While most user-facing tools are 
not yet finished, most of core changes are already done. So if you are 
a developer willing to work on some Qubes feature, we heartily 
recommend taking a look at this version. 

[qubes-31-rc1-announced]: /news/2015/12/08/qubes-OS-3-1-rc1-has-been-released/
[qubes-40-tp-release]: https://groups.google.com/d/msgid/qubes-devel/20151224015122.GA14873%40invisiblethingslab.com
[pv-grub-doc]: /doc/managing-vm-kernel/#tocAnchor-1-3
[release-notes]: /doc/releases/3.1/release-notes/
[download]: /downloads/
