---
layout: post
title: "Windows integration work in Qubes 4.1 by the tabit-pro team"
categories: articles
author: Ivan Kardykov
---

_**Editor's note:** This is a guest article by Ivan Kardykov from
[tabit-pro](https://tabit.pro/). We've invited Ivan to explain the work
the tabit-pro team contributed to Qubes 4.1. Welcome, Ivan!_

In this article, I'll briefly describe the code contributions we made to
the latest Qubes 4.1 release, most of which focus on improving Windows
integration.

## OEM activation support 

When Windows comes preinstalled on a computer, license activation is
based on a certificate embedded in the hardware. Technically, this uses
one of the ACPI tables called "SLIC," which is readable by the host OS.
This option is not available in Qubes OS, since each qube is a Xen
virtual machine (VM) that has no physical hardware of its own. However,
a small change in Xen allowed us to copy the necessary data onto the
appropriate memory partition of the VM (thanks to OpenXT for the working
patch). This can be done simply by [extending the VM configuration with
the SLIC data via the libvirt template
extension](https://github.com/QubesOS/qubes-issues/issues/5279#issuecomment-525947408).
This fix has been included in stable packages for a long time. In fact,
it is also available for Qubes 4.0 users.

## Audio support

Audio virtualization in Qubes OS is based on communication between
PulseAudio services running in each VM, including dom0 (described in
more detail [here](/doc/audio-virtualization/)). Unfortunately, this
method is problematic in the case of Windows VMs due to the lack of
PulseAudio support in Windows, and attempting to support it
independently seems like too time-consuming of a task (see
[#2624](https://github.com/QubesOS/qubes-issues/issues/2624)).

An alternative is to use QEMU, which allows for the emulation of
different audio devices and docking with PulseAudio. In our case, the
main obstacle for this method is the complexity of the connection to
QEMU, which is isolated by a stubdomain. We developed a patch that
allows for building and starting the necessary components for PulseAudio
in a minimal environment with vchan support. We also worked out a
separate version for building the stubdomain image with extended
functionality (the `xen-hvm-stubdom-linux-full` package). This mode is
activated in HVMs by setting the `audio-model` feature (using
`qvm-features`) and specifying the type of audio device, for example,
`ich6`. (Device variants are described in the QEMU documentation.)

## USB support

We used a similar approach to improve support for USB devices. In the
extended stubdomain, we proposed including qrexec and USB-proxy services
and including libusb features in QEMU (see
[qubes-app-linux-usb-proxy](https://github.com/QubesOS/qubes-app-linux-usb-proxy)).
As a result, we didn't even need to increase the available memory in
order to achieve stable operation of the extended stubdomain, although
it may be a problem when using some devices (e.g., webcams). There was a
question of which controller type QEMU should emulate. After
experimenting with different devices, we settled on the NEC XHCI, in
part due to the availability of Windows 7 drivers. The activation of
this emulation mode in HVMs is done by setting the `stubdom-qrexec`
property (using `qvm-features`). Details of user testing can be found on
the [Qubes
Forum](https://forum.qubes-os.org/t/windows-usb-integration-with-r4-1/5001).

## Not only Windows

The advantage of this approach is that there is no need to install guest
tools for attaching audio and USB devices, which allows the emulation to
be used not only with Windows, but with any OS (e.g., Linux live images,
Android x86, and probably ReactOS).  At the same time, there are also
disadvantages. In particular, the additional workload can slow down the
VM and affect the sound quality. (Even at minimum workload, you may
notice crackling.)

## Qubes Windows Tools

A relatively long time ago, we proposed building all the components of
Qubes Windows Tools (QWT) in a Linux environment using MinGW and Wine,
which allows us to use our existing CI tools and simplify the
maintenance of changes. We also worked a lot on improving the stability
of all the components, in particular, eliminating the causes of freezes
and performance degradation. In recent weeks, all of our proposed
changes have been approved and merged, and the
[cross-build](https://github.com/QubesOS/qubes-windows-tools-cross)
project has been added as a Qubes OS component. I'm sure everything will
be available in the stable repositories soon.

## Conclusion

The result of our work is full integration of Windows 7, 10, and 11 in
Qubes OS and significant improvements in usability for even
inexperienced users.

Thanks to all members of the tabit-pro team, especially Dmitriy Fedorov
([@easydozen](https://github.com/easydozen)). Many thanks to Marek
Marczykowski-Górecki for his assistance and the Qubes dev team as a
whole for their daily work.  Special thanks to the Qubes community,
especially for their kind words of support.

Ivan Kardykov  
[tabit.pro](https://tabit.pro)
