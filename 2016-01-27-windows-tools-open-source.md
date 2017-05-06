---
layout: post
title: "Qubes Windows Tools are now open source!"
date: 2016-01-27
author: Rafał Wojdyła
categories:
  - announcements
  - windows
---

The state of Windows support within Qubes
=========================================

In this blogspot we dive deep into Windows support in Qubes, announce opening the source of the Qubes Windows Tools that enables this integration, and encourage others to contribute towards the effort.

What are Qubes Windows Tools?
=============================

[Qubes OS](https://qubes-os.org/) supports [Windows AppVMs](https://www.qubes-os.org/doc/windows-appvms/) and starting from Release 2 Beta 3 it includes special [Windows guest tools](https://www.qubes-os.org/doc/windows-tools-3/) that provide better integration of the guest OS with the host system. Qubes Windows Tools (QWT for short) consist of many components:

- Base drivers for accessing various Xen-provided interfaces like event channels, memory sharing and XenStore.
- Xen paravirtualization (PV) drivers for virtual disk and ethernet devices. These provide better performance (benchmarks show a read throughput increase of 5x compared to emulated disks) and more features (dynamic attaching of external block devices for example).
- Qubes [`qrexec`](https://www.qubes-os.org/en/doc/qrexec/) agent that is responsible for communication with dom0 and handles command and service requests from other domains (remote execution, secure file transfer, clipboard sharing etc).
- Qubes GUI agent that is responsible for managing the guest virtual display and implementing a [seamless GUI experience](https://www.qubes-os.org/screenshots/).
- Miscellaneous other small components like services for managing network configuration or HVM's private disk if it's template-based.

Qubes Windows Tools and Xen PV drivers
======================================

Windows Xen PV drivers are needed for QWT to function because `libvchan`, our inter-VM communications library, depends on the low-level Xen APIs ([event channels](https://wiki.xen.org/wiki/Event_Channel_Internals), [memory sharing ](https://wiki.xen.org/wiki/Grant_Table)and [XenStore](https://wiki.xen.org/wiki/XenStore)).

History
-------

Qubes OS R2 used [GPL Windows PV drivers](https://wiki.xenproject.org/wiki/XenWindowsGplPv) written by James Harper with [additions by Chris Smowton](https://github.com/smowton/xen-pv-windows-evtchn) (user mode interfaces). In Qubes OS R2 [qrexec](https://www.qubes-os.org/en/doc/qrexec2-implementation/), our high-level protocol for inter-VM command/service execution, required that all data pass through dom0, even in case of a VM-VM connection. This detail is important because the GPLPV drivers didn't implement the client side of Xen interfaces (event channels and memory mapping). Windows guests could only open unbound event channels and grant memory to other domains. They couldn't bind to event channels in other domains or map memory from other domains.

In Qubes OS R3 the qrexec protocol was [redesigned](https://www.qubes-os.org/en/doc/qrexec3-implementation/) to allow direct VM to VM connections (after prior dom0 arbitration of course). This fact meant that the GPLPV drivers wouldn't work: as mentioned, they lacked the client-side side implementation of needed Xen APIs.

Transition to the new PV drivers
--------------------------------

We managed to create a prototype of the needed functionality just as Qubes OS R3 became stable enough to test our changes. And then, surprise: the GPLPV drivers didn't work even without the changes, the mass storage driver wasn't correctly dealing with PV disks and the OS wouldn't boot. This was most likely caused by R3 using a newer Xen version (4.4 as opposed to R2's 4.1). We haven't investigated the issue in detail because an alternative appeared: a [new Windows PV drivers initiative](https://www.xenproject.org/developers/teams/windows-pv-drivers.html) officially supported by the Xen project. These new drivers were originally part of Citrix's XenServer product but were released under a BSD license.

The new drivers worked fine under Qubes OS R3. There was a problem though -- they, too, didn't implement the client-side Xen APIs needed for us. There wasn't even a user-mode library for interfacing with the drivers. So, a decision had to be made: do we try to make the old GPL drivers work under R3, or implement all the missing bits in the new drivers. In the end we went with the new drivers and here's why:

- Quality of their code was much higher than the old drivers (at least in our opinion).
- The old drivers were pretty much a direct port of Linux code to Windows, and a lot of Windows kernel concepts are much different from Linux, especially how asynchronous I/O is done. The new drivers were written for Windows from scratch and were much easier to understand for someone who had prior experience with Windows drivers.
- The old drivers were forked by us and our repo was not synchronized with the upstream code for a long time. James Harper, the original author of the GPL drivers, continued to develop them for some time -- including a complete rewrite of the storage drivers -- after the point of our fork. It would be very complicated to merge these upstream changes into our code base. Lesson learned for the future -- if you fork, keep your version in sync, preferably by upstreaming changes.
- The old drivers are not maintained anymore -- at the time of this writing [the last commit is over a year old](https://xenbits.xensource.com/ext/win-pvdrivers). This was the most important reason to choose the new drivers.

Recent development and upstreaming efforts
==========================================

As the QWT developer I didn't need to deal with low-level Xen interfaces before. Documentation on the subject is sparse to say the least, you need to basically read the public header files to get any useful details. And even then there are many subtle details that you have no idea about if you haven't already dealt with that stuff before. It took us about a month to develop a working prototype and another one to get it into a relatively stable state (not counting later bugfixes and improvements).

Two out of five repositories in the original PV drivers project were forked:

- The XenBus driver (exports kernel-mode interfaces to other drivers)
- The XenIface driver (exports user-mode interfaces)

A DLL (`xencontrol`) that talks to the XenIface driver was added so user-mode libraries and applications could actually use all the Xen APIs.

After porting our Windows Tools to use the new libraries it was time to upstream our changes in the PV drivers. Paul Durrant, the lead of the project, was very helpful during the development and agreed to incorporate our code into the official repositories. He provided detailed feedback for the patches and improvement suggestions. XenBus patches [were](https://xenbits.xen.org/gitweb/?p=pvdrivers/win/xenbus.git;a=commit;h=586415551f759089e136a75924aca4e4448cffe4) [accepted](https://xenbits.xen.org/gitweb/?p=pvdrivers/win/xenbus.git;a=commit;h=5096ab7a917f0ea3892c00da8b7119a2d728767d) after one round of feedback. The process took two weeks and additionally we were able to find [some](https://xenbits.xen.org/gitweb/?p=pvdrivers/win/xenbus.git;a=commit;h=07736d005a77c94679fd1671820d576454b22016) [bugs](https://xenbits.xen.org/gitweb/?p=pvdrivers/win/xenbus.git;a=commit;h=8b332a8ee5912d72cba40ac2a66741d7e64eb424) in existing driver code.

Upstreaming XenIface took a lot more effort since it contains the bulk of new functionality. After two rounds of feedback and some significant [architectural](https://github.com/QubesOS/qubes-vmm-xen-win-pvdrivers-xeniface/commit/fa2a1d01b5c78ad69f7e776fdab4cae43f86c1de) [changes](https://github.com/QubesOS/qubes-vmm-xen-win-pvdrivers-xeniface/commit/93d8a6ab93182bd7fe0df00a94080e7e493502cd) the kernel-mode portion of XenIface changes [was](https://xenbits.xen.org/gitweb/?p=pvdrivers/win/xeniface.git;a=commit;h=f61c175a3a1fc6809fd71299dc863f1db9d9040f) [incorporated](https://xenbits.xen.org/gitweb/?p=pvdrivers/win/xeniface.git;a=commit;h=c7e6df4c5120ee38cb52027df505ddd1fcd79414) into the upstream repository.

The user-mode portion (xencontrol DLL and libvchan) upstreaming is still pending, mostly because of an issue with code duplication. libvchan [is now a core Xen library](https://xenbits.xen.org/gitweb/?p=xen.git;a=tree;f=tools/libvchan). The Windows port differs from Xen code mainly in calls to Xen APIs -- although xencontrol's API is somewhat based on Xen's [libxc](https://xenbits.xen.org/gitweb/?p=xen.git;a=tree;f=tools/libxc), there are some large differences (Windows vs Linux API conventions, different OS constructs etc). It would be nice to have the Windows libvchan code in upstream Xen but it's not that simple of course. Should Windows libvchan be built as a part of Xen or a part of PV drivers? Details are still to be decided.

Opening the source and the future
=================================

In the past, Qubes Windows Tools were mostly closed-source as its
development was focused on business use. However we have decided that it
is better to provide the full functionality of Qubes for all users, so
we're doing just that -- please enjoy Qubes Windows Tools with the
same confidence you use Qubes OS! [1]

What next? The main priority right now is adding support for Windows 8 and 10. Why is this a big problem? Without getting too technical, Windows 8 and later [requires](https://msdn.microsoft.com/en-us/library/windows/hardware/ff570593(v=vs.85).aspx) that video drivers use a totally different (and much more complex) architecture than what was still supported in Windows 7. The GUI agent in QWT relies on a custom video driver to provide the seamless experience for Windows VMs so said video driver needs to be ported to the new architecture. It's not simple, but progress is being made.

Another thing is to solve the code duplication issue in the user-mode portion of the Xen PV drivers so we can stop having our forked versions. It would also be nice to convince Xen maintainers to accept the Windows libvchan port.

And, last but not least, there is still a lot of work to be done to improve Qubes Windows Tools, like for example adding audio support. There are some problems with the Xen PV storage driver that we haven't had time to investigate in detail. The GUI agent could use an optimization pass. So, now that we are opening the source of QWT you can also help if you are a Windows developer! :)

[1] The source code license for Qubes Windows Tools is GNU General Public License version 2.
