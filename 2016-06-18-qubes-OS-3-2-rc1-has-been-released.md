---
layout: post
title: "Qubes OS 3.2 rc1 has been released!"
date: 2016-06-18
categories:
  - releases
download_url: /downloads/
author: Marek Marczykowski-Górecki
---
Today we're releasing the first release candidate of Qubes 3.2. This release,
similar to 3.1, is mostly focused on hardware compatibility. But as usual,
there are also some new features. Major highlights for this release are:

1. Major enhancements to management/pre-configuration. Now, it is not only
   possible to create VMs and set their properties, but you can also configure
   the insides of systems! This enables many more interesting use cases, like
   setting up new customized templates automatically. We don't have any
   finished, out-of-the-box configurations using this new feature yet, but we
   already have some preliminary versions of some of them, for example:

   * [Installing Martus][salt-martus]
   * [Updating all the templates at once][template-update]

2. Dom0 updated to Fedora 23. This provides much newer graphics drivers.
   Together with Linux kernel 4.4.x in dom0, it should make Qubes OS much more
   compatible with new hardware. With Fedora 23 we were forced to upgrade KDE 4
   to KDE 5. This is mostly done, but there are still some rough edges.
   Hopefully, we'll sort this out before final Qubes 3.2 release.

3. [USB passthrough][usb-passthrough]. For a long time it was only possible to
   attach the whole USB controller to a selected VM. With Qubes 3.2, we
   introduce USB passthrough, which allows connecting individual USB devices to
   any VM. This is actually our second attempt to do this - the previous one was
   based on Xen-specific PV USB drivers and turned out to be a failure, as those
   drivers were buggy and not maintained anymore. The current approach is based
   on the USB IP driver, which is supported in vanilla Linux.

   While this feature enables many new of possibilities, it should be noted that
   connecting a whole USB device to VM exposes that VM to all the [problems with
   USB devices][usb-challenges]. So, if there are other options for using a
   specific device (for example `qvm-block` for block devices), those
   alternatives should be used.

Read the [release notes][release-notes] for more details and installation
instructions. The installation image can be downloaded from [here][download].
A word of caution: we haven't prepared the upgrade path yet, so the only option
to try 3.2-rc1 is to do a clean reinstall (or install it on another
drive/partition).

[release-notes]: https://www.qubes-os.org/doc/releases/3.2/release-notes/
[download]: https://www.qubes-os.org/downloads/
[salt-martus]: https://gist.github.com/marmarek/29f9a4a1f3a7a457cf2b449ab0b0e2f4
[template-update]: http://article.gmane.org/gmane.os.qubes.user/784
[usb-passthrough]: https://www.qubes-os.org/doc/usb/#tocAnchor-1-1-5
[usb-challenges]: http://blog.invisiblethings.org/2011/05/31/usb-security-challenges.html
