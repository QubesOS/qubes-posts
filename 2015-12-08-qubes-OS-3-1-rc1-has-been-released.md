---
layout: post
title: "Qubes OS 3.1 rc1 has been released!"
date: 2015-12-08
categories:
  - releases
download_url: /downloads/
author: Marek Marczykowski-Górecki
---
Today we're releasing the first release candidate of Qubes 3.1. This release is
focused on usability and hardware compatibility. As [previously
announced][qubes-30-announced], the most notable features include:

1. Management/pre-configuration stack. Configuration complexity is one of the
   major UX pain points in Qubes OS. This feature is going to make it really
   simple to enable even the most complex configurations with just a few clicks.
   In Qubes 3.1 we are providing management stack for basic VM management (like
   creating VMs, setting properties there, etc). In the near future we'll
   enhance it to configure also templates (installation of additional software,
   keeping them up to date, modifying configuration files, etc).

   The 3.1 installer already uses this new mechanism for:

   * Basic VMs creation (sys-net, sys-firewall, personal, vault, etc)
   * [Whonix][whonix] setup (both workstation and gateway, with all required
           settings)
   * USB VM, including handling of USB mouse

   We are going to write a separate, much more detailed blog post about this
   major feature.

2. UEFI support: the same installation image can be started in UEFI or legacy
   mode. Depending on the mode in which the installer is running - the same mode
   will be used for the installed system. This reduces the requirements to
   run Qubes (legacy boot), lowering the barrier of entry to use Qubes.

3. A bunch of updates for improving hardware support further: Xen 4.6, updated X
   server drivers, kernel 4.1. Having those updates makes is possible to run
   Qubes smoothly on the Intel 5th gen CPUs (Broadwell).

4. [PV Grub2][pv-grub-doc] to ease custom kernel installation in VMs, and
   especially in NetVMs. This makes it now easy to install a kernel for a VM
   from an upstream repository (e.g. from Fedora or Debian), or even a manually
   compiled one. This also includes kernel modules - for example from rpmfusion
   repository. While the PV Grub(2) was supported by Xen for years, we were
   postponing its adoption because of our plans to implement the so called
   Untrusted Storage Domain.  However, given the disappointing state of trusted
   boot technologies on x86 platforms, we have recently decided to give up on
   this idea for the foreseeable future.

5. Default Fedora template updated to Fedora 23.

Read the [release notes][release-notes] for more details, installation and update
instructions. The installation image can be downloaded from [here][download].
A word of caution: we haven't tested the upgrade path yet, so make sure to do a
backup first.

Also today we've made available Fedora 23 template to download for Qubes 3.0.
You can either [update your existing Fedora 21 template][template-upgrade]
straight to Fedora 23, or install a fresh one as usual:

    qubes-dom0-update qubes-template-fedora-23

In the coming days we'll make available a live edition of Qubes 3.1-rc1
as well.

Also, today we are pleased to announce the official Qubes blog
which is located in our a newly redesigned website. This blog
will be the place for announcements, releases and interesting
things about Qubes OS.

[qubes-30-announced]: https://blog.invisiblethings.org/2015/10/01/qubes-30.html
[whonix]: /doc/templates/whonix/
[pv-grub-doc]: /doc/managing-vm-kernel/
[release-notes]: /doc/releases/3.1/release-notes/
[download]: /downloads/
[template-upgrade]: /doc/templates/fedora/#upgrading
