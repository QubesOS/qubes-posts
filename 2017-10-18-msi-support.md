---
layout: post
title: MSI support for PCI device pass-through with stub domains
categories:
 - articles
 - security
author: Simon Gaiser (aka HW42)
---

Introduction
------------

In this post, we will describe how we fixed MSI support for VMs running in HVM mode in Qubes 4.0.
First, allow us to provide some background about the MSI feature and why we need it in the first place.

In Qubes 4.0, we switched from paravirtualized (PV) virtual machines to hardware virtual machines (HVMs, also known as "fully virtualized" or "hardware-assisted" VMs) for improved security (see the [4.0-rc1 announcement][rc1-announce] for details).
For VMs running as HVMs, Xen requires software that can emulate hardware (such as network cards) called QEMU in order to provide device emulation.
By default, Xen runs QEMU in the most trusted domain, dom0, and QEMU has quite a large attack surface.
Running QEMU in dom0 would jeopardize the security of Qubes, so it is necessary to run QEMU outside of dom0.

We do this by using a Xen feature that allows us to run QEMU inside a second "helper" VM called a "stub domain".<sup>\*</sup>
This way, an attacker who exploits a bug in QEMU will be confined to the stub domain rather than getting full access to dom0.
Admittedly, stub domains run in PV mode, which means that an attacker who were to successfully exploit QEMU would gain the ability to exploit potential Xen bugs in paravirtualization.
Nonetheless, we believe using HVMs to host PCI devices is still a considerable improvement.
Of course, in the long term, we would like to switch to using PVH VMs, but at the moment this is not feasible.

In our testing, we found that pass-through PCI devices did not work in HVMs on some machines.
On the affected machines, networking devices and USB devices, for example, were not usable as they are in Qubes 3.2.
(The kernel driver failed to initialize the device.)
This was a major problem that would have blocked us from moving entirely from PV to HVM in Qubes 4.0.
For this reason, the Qubes 4.0-rc1 installer configures all VMs that have attached PCI devices to use PV mode so that those PCI devices will function correctly.

Problems
--------

After much further testing, we discovered that the affected PCI devices don't work without MSI support.
(MSI is a method to trigger an interrupt from a PCI device.)
The devices we observed to be problematic were all newer Intel devices (integrated USB controllers and a Wi-Fi card).
While the PCIe standard allows for devices that don't support legacy interrupts, all the affected devices advertised support for legacy interrupts.
But no interrupts were ever delivered after the driver configured the device.
This made the bug tricky to track down, since we were looking for an error on the software side.

To get those devices working, we needed MSI support.
When running QEMU in dom0, MSI support (and therefore the problematic devices) worked, but with stub domains, it was broken.
This is why, until now, we've had patches in place to hide MSI capability from the guest so that the driver doesn't try to use it ([one patch for the Mini-OS-based stub domain][msi-cap-disable-minios] and [another for the new Linux-based stub domain][msi-cap-disable-linux]).

We found two issues that were preventing MSI support from working with stub domains.
First, the stub domain did not have the required permission on the IRQ, which is reserved for the MSI in the `map_pirq` hypercall QEMU makes.
(The IRQ is basically a number to distinguish between interrupts from different devices.)
Fortunately, this problem had already been tracked down by [OpenXT][openxt], and they made a patch for it (the [original][irq-allow-orig] and our [pull request][irq-allow-qubes] based on their patch).

The second problem was that, after setting MSI up, it needed to be enabled.
This is done by setting the MSI enable flag in the PCI config space, which is a special memory mapped region used to configure a PCI device.
However, this write did not reach the device, and therefore no interrupts were delivered.
When running in dom0, the config space write from QEMU goes directly to the real PCI device.
By contrast, inside a stub domain, the write goes to the pcifront driver inside the stub domain and is then blocked by the pciback running in dom0.
With a test patch, we verified that the only problem was with the enable flag write.
No other config space writes were problematic.

Solutions
---------

It appeared that we had to choose from the following options:

1. Enable permissive mode.
   In permissive mode, pciback allows (almost) all writes to the PCI config space.
   This seems to be the solution OpenXT chose (see [this issue][enable-issue-openxt] and [this commit][enable-commit-openxt]).
2. Allow just the write to this specific config space location (i.e. the enable flag).
   Unlike option (1), this option entails allowing only the required write and no others.
3. Since pcifront has the ability to issue a specific command to pciback to enable MSI, maybe QEMU should send this command instead of the write to the config space.
4. Something else.
   For example, maybe the MSI config should not be handled by QEMU at all in the case of stub domains but should instead be handled directly by Xen.

Option (3) didn't appear promising.
The enable command that pcifront sends is intended for the normal PV use case where the device is passed to the VM itself (via pcifront) rather than to the stub domain target.
While the command is called `enable_msi`, pciback does much more than simply setting the enable flag.
It also configures IRQ handling in the dom0 kernel, adapts the MSI masking, and more.
This makes sense in the PV case, but in the HVM case, the MSI configuration is done by QEMU, so this most likely won't work correctly.

Option (1) would have been the easiest solution.
We would just need to set the option in the domain config.
After discussing it, however, we weren't convinced that this option is safe (but we also don't claim it isn't).
See the paper discussed below and this [thread][permissive-thread] for some potential problems.

So, what about option (2)?
We had to think about whether this might enable a new attack.
If we were to implement option (2), the security scenario would be different from the scenario in which QEMU runs in dom0.
When QEMU runs in dom0, it ensures that MSI is configured in a certain way before enabling MSIs ([details][qemu-msi-enable-write]).
However, we've put QEMU in a stub domain so that we don't have to trust it.
This means that we can no longer trust it to ensure that MSI is configured safely.
What would happen if, for example, a malicious stub domain were to set the enable flag of a PCI device without first configuring it?
As it turns out, ITL has published research relevant to assessing this risk.

In ["Following the White Rabbit: Software attacks against Intel(R) VT-d technology"][paper], Rafał Wojtczuk and Joanna Rutkowska describe an attack against VT-d on machines without interrupt remapping support.
For our purposes, the result they describe on page 8 is very important:
Even without access to the PCI config space, a malicious guest is, in many cases, able to generate arbitrary MSIs.
So long as writing to the MSI enable flag does not have any unrelated side effects, there's no obvious way in which allowing it can worsen security, since an attacker who can set it can already generate arbitrary MSIs anyway.
Meanwhile, we reap the benefits of using HVMs to better isolate VMs with attached PCI devices.

So, we decided to implement option (2).
Based on the analysis above, one could argue that we might as well allow writes to the enable flags for all VMs with attached PCI devices, since doing so shouldn't decrease security.
To be extra cautious, however, we only allow writes to the enable flags for stub domains.
In other cases, it's not necessary.
(Here are our patches for [pciback][patch-pciback] and [libxl][patch-libxl].)

Now, the previously problematic devices function correctly inside HVMs.
(Here are the full pull requests: [1][pr-linux-kernel], [2][pr-vmm-xen], [3][pr-vmm-xen-stubdom-linux].)
We just merged this feature, and it will be included in Qubes 4.0-rc2, which we plan to release next week.
After these patches undergo further testing, we plan to upstream them so that all Xen users can benefit from our work.

If you have any questions or comments, please write to us on [qubes-devel][qubes-devel].

<sup>\*</sup> <small>We've switch from the Mini-OS-based stub domain to a Linux-based stub domain in Qubes 4.0 based on [patches][linux-stubdom-orig-patches] from Anthony Perad and Eric Shelton.
The switch is not significant for the purposes of this article.</small>

[rc1-announce]: /news/2017/07/31/qubes-40-rc1/#fully-virtualized-vms-for-better-isolation
[msi-cap-disable-minios]: https://github.com/QubesOS/qubes-vmm-xen/blob/ff5eaaa777e9d6ba42242479d1cabacfbdc728ca/patches.misc/hvmpt02-disable-msi-caps.patch
[msi-cap-disable-linux]: https://github.com/QubesOS/qubes-vmm-xen-stubdom-linux/blob/71a01b41a9cf69d580c652a7147c0a8eb33ced97/qemu/patches/disable-msi-caps.patch
[openxt]: http://openxt.org/
[irq-allow-orig]: https://github.com/OpenXT/xenclient-oe/blob/5e0e7304a5a3c75ef01240a1e3673665b2aaf05e/recipes-extended/xen/files/stubdomain-msi-irq-access.patch
[irq-allow-qubes]: https://github.com/QubesOS/qubes-vmm-xen/pull/15/commits/2a5229f24296347a40ba3250465a61ca425a6146
[enable-issue-openxt]: https://openxt.atlassian.net/browse/OXT-894
[enable-commit-openxt]: https://github.com/OpenXT/manager/pull/52/commits/5950ebe73f2411f3af37f5dd56c5c70619e5d99f
[qubes-devel]: /support/#qubes-devel
[qemu-msi-enable-write]: https://git.qemu.org/?p=qemu.git;a=blob;f=hw/xen/xen_pt_config_init.c;h=6f18366f6768ee3d7b72f588dc990a6329124a04;hb=359c41abe32638adad503e386969fa428cecff52#l1114
[paper]: https://invisiblethingslab.com/resources/2011/Software%20Attacks%20on%20Intel%20VT-d.pdf
[patch-pciback]: https://github.com/QubesOS/qubes-linux-kernel/pull/12/commits/96b956b38cb24230848a563d3e1ce359c8d8db66
[patch-libxl]: https://github.com/QubesOS/qubes-vmm-xen/pull/15/commits/55ef595451d9e2e5583a31c4a3600507ae5500f7
[linux-stubdom-orig-patches]: https://lists.xenproject.org/archives/html/xen-devel/2015-02/msg00426.html
[permissive-thread]: https://lists.xenproject.org/archives/html/xen-devel/2010-07/msg00257.html
[pr-vmm-xen]: https://github.com/QubesOS/qubes-vmm-xen/pull/15
[pr-linux-kernel]: https://github.com/QubesOS/qubes-linux-kernel/pull/12
[pr-vmm-xen-stubdom-linux]: https://github.com/QubesOS/qubes-vmm-xen-stubdom-linux/pull/3
