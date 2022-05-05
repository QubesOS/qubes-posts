---
layout: post
title: "Automated OS testing on physical laptops"
categories: articles
author: Marek Marczykowski-Górecki
---

Our journey towards automating OS tests on physical laptops started few years
ago with the idea of using Intel AMT to drive tests on physical machines. To
start, I got an [initial
implementation](https://github.com/os-autoinst/os-autoinst/pull/983) working.
In particular, VNC for input/output and power control worked. I tried to get a
virtual CD working, but it turned out to be quite unstable. Worse --- and more
importantly --- it was really just a CD, not a CD/DVD, which meant that the
protocol couldn't handle images larger than 2GB. Some time later I abandoned
this approach, for two related reasons:

1. Many machines that we want Qubes OS to support intentionally do not have
   Intel AMT.
2. The single AMT-enabled machine that I had been using to develop this feature
   broke.

If anyone would like to resume this work, [this
page](https://web.archive.org/web/20200926070850/senseless.info/amt.html)
includes _a lot_ of useful info about Intel AMT on Linux.

Recently, I came back to the project with a new approach: to capture video from
HDMI output and use an emulated USB keyboard and mouse for input. Then, I added
power control to the mix, combined everything on a Raspberry Pi, and got a
[working prototype](https://github.com/os-autoinst/os-autoinst/pull/1741) of an
openQA worker that runs the tests on a physical machine, instead of a virtual
one.

The whole setup includes several devices:

- One "central" Raspberry Pi that controls a power strip and serves boot files.
- One Raspberry Pi per laptop that runs an openQA worker for that laptop. It
  emulates a USB device for that laptop and captures HDMI output from it.

All these elements are detailed below.

## Base system

The goal was to run an openQA worker on a Raspberry Pi 4. Why a Raspberry Pi
(RPi)?

- Their USB controllers can play the role of a device, not just that of USB
  host.
- They're powerful enough to run the video processing required by openQA.
- They're (mostly) readily available and relatively cheap.

As a base system, I chose OpenSUSE, because that's openQA's native
distribution. Getting OpenSUSE to work on an RPi was [rather
straightforward](https://en.opensuse.org/HCL:Raspberry_Pi4), but the choice did
lead to few issues mentioned later in this article.

## Power control

Power control was the first stage of this project. I thought it looked like the
simplest part.

To reliably run unattended tests, I needed a way to interrupt a test when it
went into some unrecoverable state (kernel panic, hard hang, etc.). With AMT, I
had a built-in API for that, but now I needed something else. I chose a power
strip that was remotely controlled via USB. Then, I removed the batteries from
the laptops connected to the setup. This gave me a very reliable way to
interrupt whatever was running on the machines by simply powering them down.
But it turned out that powering them back on may not be that simple.

In the current setup, there are several laptops, each of them slightly
different, and each (sic!) requiring a slightly different approach to power
management. Here are some things I tried and that worked on some machines:

1. Setting the BIOS to automatically power on the machine when a power supply
   was connected. This is the simplest method. Sadly, only one of the machines
   supported it.
2. Sending a Wake-On-Lan packet. Here, reliability depends on the device. For
   some, it just works, while others require enabling it in the network card
   (`ethtool -s eth0 wol g` command), and some lose the setting either on
   system startup or on disconnecting the power...
3. When all else fails, one can just press the physical power button. Of course
   it would be too much work to do it manually, so I attached a servo motor in
   the exact spot where the power button is. Then, I drive that servo motor
   from an RPi.

[![Power button servo](/attachment/posts/openqa-hw-power-button.jpg)](/attachment/posts/openqa-hw-power-button.jpg)

## System startup

After achieving control over system power, the next step was taking control of
which operating system starts there. I considered two options:

- A USB boot drive, emulated from an RPi
- Network boot

The first option turned out to be problematic when combined with emulated USB
input devices (see below), at least on some laptops. While a single USB device
can have multiple interfaces (basically being sub-devices), many system
firmwares do not like to boot from such devices. When I exposed a USB device
that has both a storage interface and a HID interface (keyboard/mouse), the
system didn't consider it a bootable device. One solution would be to use two
separate devices, but that would require yet another RPi (or something
similar), since most (all?) such boards support emulating only a single device.
Another way around it could be emulating a USB hub and getting two virtual
devices this way, but Linux does not support USB hub emulation. Since I had an
alternative, I didn't explore this option any further. On systems that are fine
with a single multi-function USB device, I can use that. On others, I use
network boot.

The second option turned out not to be that straightforward either. First of
all, not all systems support booting from the network to begin with. To solve
this problem, I got a USB stick and put [iPXE](https://ipxe.org) on it. Then, I
configured the system to boot from that USB stick. I couldn't use Grub here to
gain network boot, because Grub supports only network devices via the system
firmware (BIOS/EFI) support, and this support is missing on systems not capable
of network booting. iPXE, on the other hand, supports a wide range of network
devices on its own, and also allows simple scripting, like booting different
systems depending on various settings. Unfortunately, it cannot boot Xen via
the multiboot2 protocol (required to boot with full EFI support), it can only
do multiboot1. So, I did need Grub. Luckily, iPXE does register its drivers as
appropriate EFI services, so when I load Grub from iPXE, it can talk to the
network.

I prepared a Grub configuration that can boot different systems on different
laptops depending on a separate configuration file (loaded via the `load_env`
grub command) and a tool to conveniently switch between them. This got me a
nice menu:

    $ testbed-control 2 help
    Selected target: 2

    Available commands:
     - reset - hard reset the target
     - poweron - power on the target
     - poweroff - (hard) power off the target
     - wake - wake up the system (either wake-on-lan, or button press)
     - rescue - switch next boot to rescue system (doesn't load anything from the disk)
     - fallback - switch next boot to fallback system (loads /boot/efi/EFI/qubes/grub-fallback.cfg)
     - normal - switch next boot to normal system
     - custom - switch next boot to custom grub config (/srv/tftp/test2/grub.cfg)

The first four commands are about power control (see above), and the rest are
about choosing what to boot. The `normal` command simply starts the system
installed on the local disk,  while `rescue` allows booting an initramfs-only
system to diagnose why the normal system doesn't work. The `custom` option
allows, in practice, starting an arbitrary kernel (not necessarily from the
disk). That option is especially useful for debugging Linux and Xen issues,
including doing automatic bisection, although it requires a bit more in terms
of glue scripts (but that's a topic for another article).

Surprisingly, I had one case where booting the local system turned out to be
tricky. When the bootloader is loaded from the network, that particular UEFI
does not register services to access the local disk. As it turns out, Grub does
not support NVMe drives directly; it supports them only via UEFI services. I
could have switched to another disk, or to booting via USB, but neither of
those options felt appealing. I wanted to run tests on NVMe drives too, and
while USB booting works, it is a bit fragile, because one needs to be careful
not to overwrite that boot drive (especially when testing system
installations). So, I developed a workaround: setting a `BootNext` EFI variable
(selecting the alternative boot option for just the next startup) and
rebooting. Unfortunately, Grub itself does not have a function to set EFI
variables (it can only read them), but building Linux + minimal initrd with
relevant tools is rather easy. By the way, if I were starting Linux anyway, I
could simply kexec the target kernel from the NVMe disk using Linux's drivers,
but I wanted the actual startup to remain closer to the "normal" startup,
including respecting the  relevant Grub configuration.

There was one final problem to solve. When installing Qubes OS, it will set
itself as the default boot target. This means that all of the above boot
options will be overridden by the installer. To solve this issue, I passed a
kickstart file to the installer that restores the original boot order at the
very last step (`%post` script).

To summarize, I now had:

- A way to load Grub2 on each test system (either via PXE or via iPXE loaded
  from a USB stick)
- A way to conveniently control which OS Grub2 will start
- A way to load a local kernel even if Grub2 does not see the disk

[![Startup diagram](/attachment/posts/openqa-hw-boot.drawio.png)](/attachment/posts/openqa-hw-boot.drawio.png)

## Video capture

I started experimenting with HDMI-over-IP extenders. Some turned out to use a
rather [standard video
format](https://blog.danman.eu/new-version-of-lenkeng-hdmi-over-ip-extender-lkv373a/)
for streaming. It worked fine... with one little inconvenience: handling the
network stream put a significant load on the Raspberry Pi that handled it. I
could use a different system for video processing than the RPi responsible for
USB emulation, but that would make the whole setup even more complex. Anyway,
that's just a minor inconvenience that requires some more cooling on the RPi,
not a deal breaker.

About the time I got all of this working, I came across
[PiKVM](https://pikvm.org), which looked almost exactly like what I needed. It
uses a TC358743 chip connected directly to an RPi (via camera interface)
instead of a separate HDMI-to-IP encoder. Setting it up presented some
challenges, but the PiKVM project (or, I should say, Maxim Davaev, the guy
behind the project) had all of this figured out already.

The first issue I encountered was getting a TC358743 device initialized and
detected at all. There were several parts to this:

1. The default kernel from OpenSUSE does not include all the necessary drivers
   (especially `bcm2835-unicam`). They're currently available only in a [kernel
   from the Raspberry Pi
   Foundation](https://www.raspberrypi.com/documentation/accessories/camera.html#v4l2).
   I chose to compile it myself, with a config based on the one from the PiKVM
   project. There could be something I'm missing here, but this approach got me
   a working setup, and I didn't want to spend too much time on debugging video
   drivers.
2. Several modifications to the `config.txt` were required:
   - `dtoverlay=tc358743` --- let the kernel know where the device is
   - `start_x=1` --- load GPU firmware with video input processing included
   - `gpu_mem=128` --- required by `start_x=1`
   The latter two must be in `config.txt` specifically, not a file
   [included](https://www.raspberrypi.com/documentation/computers/config_txt.html#include)
   from there, which is a bit problematic on OpenSUSE, because the `config.txt`
   is forcefully overridden on each update and only the included
   `extraconfig.txt` is meant for user modification. I worked around the issue
   by mounting the bootloader partition under an alternative mountpoint to
   disarm the `config.txt` override. This issue is [in OpenSUSE's bug
   tracker](https://bugzilla.opensuse.org/show_bug.cgi?id=1192047). I have yet
   to test the upstream fix for the issue.

After fixing the above, I had a `/dev/video0` device. Then, it was just a
matter of [configuring
it](https://forums.raspberrypi.com/viewtopic.php?f=38&t=281972). Specifically:

1. Loading an appropriate EDID: `v4l2-ctl --set-edid=...`. The EDID describes
   the capabilities of this "monitor". There is a catch if you want to use Full
   HD resolution: the interface bandwidth is a bit too low for 1920x1080 with a
   60Hz refresh rate, but it is enough for 50Hz (yes, unfortunately). This had
   to be described in the EDID. The author of the tutorial linked above
   [provided some examples](https://github.com/6by9/CSI2_device_config).
2. Setting digital video timings: `v4l2-ctl --set-dv-bt-timings query`. This
   can be done only when the system connected to the HDMI port starts and
   chooses a resolution, and it needs to be repeated each time the resolution
   changes. 

I've [integrated](https://github.com/os-autoinst/os-autoinst/pull/1741) both of
the above into the openQA driver.

For the openQA integration, using 1920x1080 resolution was not perfect. OpenQA
operates on images at 1024x768. If it receives anything else, it scales it. The
result of a 1920x1080 screen capture downscaled to 1024x768 was not nice, to
put it mildly. It not only made some text unreadable, but the difference in
aspect ratios heavily distorted the image. For example, this made it impossible
to reuse reference images made in other tests. I am considering enhancing
openQA to support other resolutions too, but for now I have set the resolution
on the tested system to 1024x768 (and used an EDID that lists that resolution).

[![Scalled down screenshot](/attachment/posts/openqa-hw-screen-scaled.png)](/attachment/posts/openqa-hw-screen-scaled.png)

On the tested system, something needs to actually enable HDMI output. For this
purpose, I passed a kickstart file to the Qubes OS installer that includes
commands to execute before installation (the `%pre` section). While at it, I
could use the same kickstart file for other test-related customizations, like
restoring the default boot order at the end or enabling SSH access for
collecting logs.

## HID input

Recording video output is not everything. To run tests, one also needs to send
commands to the system under test (SUT). This can be done in several ways,
including via serial console and SSH connection. In order to have the most
realistic setup, I chose to emulate USB input devices. With this, we could
interact with the system in the same way a user would. To emulate USB input
device(s), I used the Linux USB Gadget subsystem. To emulate HID devices, I had
to prepare a HID descriptor --- a description for the driver, listing what kind
of device it is and what events it can send.

I wanted the device(s) to meet the following requirements:

- Have two interfaces (which in practice is two separate HID devices): keyboard
  and pointer (mouse/tablet)
- Be properly categorized by udev (so the [input
  proxy](https://github.com/QubesOS/qubes-app-linux-input-proxy) picks it up
  properly)
- Be properly categorized by Xorg
- Support both absolute pointer position events (like "move mouse to a specific
  point" instead of "move mouse a bit to the right") and normal mouse buttons

I searched for a descriptor meeting the above requirements. The one for
keyboards is rather standard, but the one for mouse/tablet devices is not. So,
I took the [Device Class Definition for HID 1.11 together with HID Usage Tables
1.22](https://www.usb.org/hid) and crafted one myself. This was a bit of a
challenge, because both udev and Xorg have a set of heuristics to categorize
devices, and they differ in subtle ways.

Then, I wrote a
[script](https://gist.github.com/marmarek/5c44ffeb2f36e106b4d34e7e8780c208)
that sets this all up and controls the device(s) according to what openQA
requests.

The last detail is about connecting an RPi to the target system. RPi4 has a
single USB-C port used both for powering the RPi itself as well as for USB
device emulation. Generally, this would be fine, with the exception that the
target system is going to be disconnected from power from time to time. If the
RPi were powered this way, it would loose the power too, and there would be
nothing capable of turning it back on. This is yet another case where the PiKVM
project provided an
[inspiration](https://github.com/pikvm/pikvm#setting-up-the-v2): a Y-split
cable that connects the VBUS pin to only one end and the data pins to the
other.

## Serial console

Several openQA functions require some kind of console access. This includes
retrieving command outputs (and exit codes), waiting for various events, etc.
Unfortunately, however, a real serial console is very rare in modern laptops. I
could restructure the tests not to use those functions, but that would be
rather disappointing in terms of test result quality. As a solution, I added a
small qrexec service in dom0 that reads a pipe that pretends to be a serial
console, then I used `qvm-connect-tcp` in `sys-net` to redirect the TCP port to
that service. This isn't as reliable as a real serial console (especially for
things like restarting `sys-net`), but it does work in the majority of cases.
In the future, I will restructure the tests not to rely on this functionality
in order to account for the few rare cases where it doesn't work.

## Bonus: remote-controlled test laptops for developers

Remote power and boot control is useful not only for automatic tests, but also
for ordinary developers. There are several cases where it is useful:

- Additional machines to develop and test features on different versions of
  Qubes
- Access to specific hardware

I've prepared the whole setup to be usable not only with openQA, but also to
allow for the delegation of specific test machines to individual trusted
developers. More importantly, this allows not only for manually testing
software on those machines, but also for automating several tasks, such as the
git bisect mentioned earlier.

## Final thoughts

Testing a whole operating system is a challenging task, because there are a lot
of moving parts. OpenQA is a great tool for that, but its main target is
running tests in a virtualized environment. This works fine for several
components (like Qubes Manager and GUI virtualization) but not for
hardware-related features (sys-net, sys-usb, system suspend, and several more).
Before this work, we ran tests on actual laptops manually, but that was
time-consuming and thus not all updates or configurations were tested.
Automation allows our testing to be much more comprehensive, including ensuring
ongoing compatibility for Qubes OS certified hardware.
