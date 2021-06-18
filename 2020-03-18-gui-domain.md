---
layout: post
title: "Qubes Architecture Next Steps: The GUI Domain"
categories: articles
author: Marek Marczykowski-Górecki, Marta Marczykowska-Górecka
image: /attachment/posts/guivm-hybrid.png
---

It has been some time since the last design post about Qubes. There have been some [big changes happened](/news/2018/10/25/thank-you-joanna/), and it took us a little while to find people best suited to write these articles. But, as you can see, the ~~victims~~ volunteers have been found. The team has been hard at work on the changes that are coming in 4.1, and we want to tell you more about them.

One of the Big Things coming soon, in Qubes 4.1, is the first public version of the GUI domain: the next step in decoupling the graphical hardware, the display and management, and the host system. Very briefly, the GUI domain is a qube separate from dom0 that handles all the display-related tasks and some system management.

## Why make a GUI domain at all?

One of the biggest security concerns at the moment for Qubes is how much power is in dom0. Once a person has access to it, they can do anything: and while we separate it quite effectively from what is running inside application qubes, dom0 is still a big, bloated and complex domain that performs many disparate functions. It handles managing other domains, display and graphical interfaces, multiple devices (including audio devices), memory and disk management, and so on.

We mitigate many of the GUI-related risks (like the powers wielded by the window manager, or the fact that huge, complex libraries such as Qt/Gtk are always an increased attack surface) through compartmentalization: Applications in VMs can't talk to GUI toolkits in dom0 other than through a very limited Qubes-GUI protocol, and GUI toolkits in application VMs can't talk directly to dom0's X server. Moreover, dom0 is responsible for drawing the colored window borders the represent trust levels, so compromised VMs can't spoof them.

Nonetheless, having a GUI in dom0 at all is, at best, a source of many dangerous temptations. It's far too easy to use it to access untrusted (and thus potentially dangerous data), for example by mounting a disk from a qube into dom0. Even browsing relaxing landscapes as desktop wallpapers can expose dom0 to numerous vulnerabilities that intermittently appear in image-processing libraries.

Furthermore, while in theory dom0 is isolated from the outside world, some graphical devices (e.g. displays connected via HDMI or DVI) offer two-way communication, which threatens this isolation and makes it harder to maintain. If a malicious device (rather than the user's trusted monitor) were to be connected to one of these ports, it could inject data that could be processed inside of dom0. As long as graphical devices are in dom0, they also cannot be safely proxied to other domains. This is because the various solutions to multiplexing access to the GPU at the GPU/driver level (which would expose the "full" GPU to a VM) are orders of magnitude more complex than running display drivers in just one place. We consider this added complexity too risky to put it in dom0. Errors in the drivers could expose dom0 to an attack, and attacks on dom0 are the biggest threat to the Qubes security model.

The current model, in which the GUI and administrative domains are both within dom0, is also problematic from a management point of view. The way existing user-based privilege control works in most modern systems is one of the reasons why we need Qubes at all: It provides far too little separation, and root exploits seem to be inescapable in a system as monolithic as Linux. Separating the GUI domain from dom0 allows us to manage its access to the underlying system.

This has obvious uses in an organizational context, allowing for (possibly remotely) managed Qubes installations, but even in a personal computer context it is often extremely useful to have multiple user accounts with truly separate permissions and privileges. Perhaps you would like to create a guest account for any friend who needs to borrow your computer for a moment, and allow that account to create Disposable VMs, but not to create normal qubes and not to access other users' qubes. It becomes possible when the GUI domain is decoupled from dom0. All kinds of kiosk modes, providing safer environments for less-technical users who prefer to be sure they cannot break something accidentally, multi-user environments --- they all become possible.

## What needs to be ready?

There were two big issues in the previous Qubes architecture that needed to be handled for an effective approach to a GUI domain: how the GUI protocol relied on dom0-level privileges and how managing anything in the system required dom0-level access to the hypervisor.

### GUI Protocol

Detailed documentation of the current GUI protocol is available [here](/doc/gui/). In brief, it consists of a GUI agent and a GUI daemon. The GUI agent runs in a qube and connects to the GUI daemon in dom0, passing a list of memory addresses of window buffers. As the GUI daemon is running in dom0, with privileged access to, well, everything, it can just map any page of any qube's memory. You can see why this might be a bit worrying: Access to memory is power, thus dom0 is all-powerful. It would be far worse if we tried to duplicate this architecture and make our GUI domain a qube with the same memory-related privileges. It would just result in two dom0s. Rather than being reduced, the attack surface would be increased.

The upcoming 4.1 release changes this protocol to a more flexible form. It will no longer use direct memory addresses, but an abstract mechanism in which the qube has to explicitly allow access to a particular memory page. In our current implementation --- under Xen --- we use the grant tables mechanism, which provides a separate memory allocation API and allows working on grants and not directly on memory pages. Other implementations will also be possible: whether for another hypervisor (e.g. KVM) or for a completely different architecture not based on shared memory (e.g. directly sending frames to another machine).

### Managing the system

The second problem --- system management --- is actually partially solved already in Qubes 4.0. Administrative actions such as creating, changing or starting qubes can be handled via qrexec calls and controlled via qrexec policy. You can read more about the Admin API, one of the big changes in Qubes 4.0 that made all this possible [here](/news/2017/06/27/qubes-admin-api/).

Currently, in Qubes 4.0, dom0 handles all these administrative actions. However, in order to avoid unpleasant surprises and to prepare the architecture for the GUI domain, we already always perform them via Admin API. At the design level, dom0 is no longer a special and different case: It makes qrexec calls like any other qube.

There's an interesting, subtle detail here: We just accepted dom0 being able to run anything in any way inside other qubes. But if we want to implement a more contained and less-privileged GUI domain, it would defeat part of its purpose to just permit it to run any sort of `qvm-run do-what-I-want` in any of the managed qubes. Qubes 4.0 introduces a special `qubes.StartApp` qrexec service that runs only applications defined inside the target qube (currently defined via `.desktop` files in Linux and `.lnk` files in Windows, placed in predetermined directories). This mechanism allows a qube to explicitly define "what the GUI domain can execute inside me," and not just hand over all the power to the managing domain. This also makes it possible to define allowed applications using the qrexec policy!

### Other issues

Actually implementing a GUI domain (more details below) revealed a lot of minor problems that require some handling. Unsurprisingly, it turns out a modern operating system encourages a very close relationship between whatever part of it deals with graphical display and all the rest of the hardware.

Power management has numerous vital graphical tools that need some kind of access to underlying hardware. From a battery level widget to laptop power management settings, those innocuous GUI tools would like to have a surprisingly broad access to the system itself. Even suspend and shutdown need special handling. In Qubes 4.0, we could just turn off dom0 and know the rest of the system would follow, but it is no longer so simple with a non-privileged GUI domain in the picture.

Keyboard and user input need to be carefully proxied to the GUI domain to enable us to actually use the system. The existing InputProxy system needs to be expanded to ferry information from the USB domain (in the case of USB keyboards and mice) and from dom0 (in some other cases, like PS/2 keyboards) to the GUI domain.

The current state of those minor (minor in comparison to broad, architecture-level changes, but by no means unimportant) issues is tracked [here](https://github.com/QubesOS/qubes-issues/issues/4186).

## How can the GUI domain actually work?

### GPU passthrough: the perfect-world desktop solution

[![GUIVM with PCI passthrough](/attachment/posts/guivm-gpu.png)](/attachment/posts/guivm-gpu.png)

In the perfect world, we could simply connect the graphics card to the VM as a PCI device and enjoy a new, more comfortable level of separation. Unfortunately, the world of computer hardware is very far from a perfect one. This solution works only very rarely. For most graphics cards, it just fails, although some success has been observed on some AMD cards. Even if, in theory, the architecture supports GPU passthrough, many implementations rely on various hardware quirks and peculiarities absent when there is no direct access to the underlying system. For example, the video BIOS (the code that the GPU provides to the system to initialize itself) in many cases assumes that it is running with full privileges and tries to access various registers and memory areas not available to (or virtualized in) VMs.

And all that is without even approaching issues with multiple graphical cards, multiple outputs or suspending the host; or the fact that some hardware manufactures (like NVIDIA) attempt to block GPU passthrough for some of their products.

At the moment, a group of very brave university students are working on the basic GPU passthrough case as their bachelor's degree project. We wish them a lot of luck in this difficult endeavor!

### Virtual server: the perfect remote solution

[![GUIVM with VNC](/attachment/posts/guivm-vnc.png)](/attachment/posts/guivm-vnc.png)

Instead of wrestling with the hardware problems, GUI domain could instead connect to a virtual graphical server such as FreeRDP or VNC. This server could be accessible from anywhere on the network (in practice, it should be secured with at least a VPN, as bugs allowing unauthorized users access could be very dangerous), allowing for a Qubes Server hosting many separate sets of qubes used by different users, still maintaining comfortable separation between the qubes and the users. Qrexec policy allows the administrator to comfortably manage this solution: Every GUI domain can have its own set of privileges, managed qubes, Disposable VM permissions etc.

Surprisingly, a virtual server solution does actually work with the current state of Qubes as of the 4.1 developer preview build, and it allows us to bypass the dreaded GPU passthrough complications. The only not-so-small problem is that it does not actually handle our main use case: Qubes running locally on a single machine. This is because it uses the network to expose the GUI, and the place where the local display is handled (dom0) doesn't have access to the network.

### The compromise solution

[![GUIVM with Xephyr](/attachment/posts/guivm-hybrid.png)](/attachment/posts/guivm-hybrid.png)

While GPU passthrough is a work-in-progress and a server-based solution is impractical, there is a compromise solution: Dom0 can keep the X Server and graphics drivers but use them to run only a single, simple application --- a full-screen proxy for the GUI domain's graphical server (an approach similar to the one used by OpenXT). We could even use VNC for this, but luckily, there is another solution based on protocols that have already been tested and implemented. Through the GUI protocol's shared memory and a Xephyr server on the dom0 side, we can achieve something of a GUI protocol nesting.

Like many compromises, it is far from completely satisfying. The biggest problem is that it still keeps clutter (in the form of drivers and X Server) in dom0 --- much less clutter given that huge libraries and desktop environments no longer need to live there, but still clutter. Many of the GPU passthrough problems are still here: Power management will require some finesse, and multi-monitor setups are still untested. (They may require us to extend some of Xephyr's functionality.)

However, we can be pretty sure there will never be a GPU passthrough solution that works on every system. It is not just about the complexity of the problem and the multitude of GPU products available. As mentioned above, some manufacturers intentionally obstruct GPU passthrough in their graphics cards, so it is likely that some hardware configurations will never have full support. This is why the compromise solution will be available as a fallback even once more robust GPU passthrough is developed.

## Surprise dependency: audio

As far as system architecture goes, audio systems are a completely separate set of processes, communication channels and tools --- but this is only the theory. In practice, audio is very tightly connected to GUI in most modern systems. On Qubes 4.0, pulseaudio tools start together with GUI tools both in dom0 and in application qubes.

While audio drivers and tools are not nearly as bloated and sprawling as GUI tools, keeping them in dom0 is still suboptimal, and with the move toward a GUI domain, it will become increasingly impossible. Our first step was to see how we could move audio away from dom0: Connect it together with the GPU to the GUI domain and see what breaks. Surprisingly, few things did, and while some hard-coded "connect to dom0 for all of your audio needs" configurations needed to be updated, those changes are already done in Qubes 4.1.

This is not the final solution we would like, though; it would be best to truly decouple audio and GUI, creating a dedicated and separate audio domain.

### Audio Domain

The audio domain will be a separate virtual machine that accesses and proxies audio card access. This way, we can not only remove audio from dom0 (making it smaller and less exposed) but also from the GUI domain (which, by virtue of being still quite privileged, should also have as little additional capabilities as possible).

All the complex audio subsystems, from pulseaudio (which controls volume for each domain) to audio mixers and microphones, would reside in the audio domain. It will have its own set of particular privileges. For example, due to the current audio hardware architecture, the audio domain will have access to the complete audio intput and output, but isolating them in a separate domain will significantly reduce the attack surface. Keeping audio in the same domain as the keyboard or screen could, in theory, lead to eavesdropping attacks. In a separate audio domain, all those potentially vulnerable devices are isolated. Even Bluetooth audio devices (like headphones) could finally be used securely, without exposing the whole system to attack.

## What will actually be in Qubes 4.1

Most of code to handle the compromise solution is either already merged into the Qubes master branch or currently awaits final merging and will be available in Qubes 4.1. However, **it will not be the default**.

The GUI domain will be an **experimental** feature. We will provide a salt formula to easily configure it for anyone who wants to try it out and play around with it. Our main goal is to test everything we can test without GPU passthrough in order to reach a state in which the aforementioned more minor problems are handled. Then we'll be ready for a GPU passthrough solution once it is developed (which is being worked on separately).

The GUI domain is currently ready for Linux-based qubes and for fullscreen HVMs, not for the Windows GUI agent. At the moment, nobody on our team is the sort of Windows wizard who could do that, so Qubes 4.1 will not have GUI domain support for for the Windows GUI agent. (Coincidentally, this is the same reason that the GUI agent is not compatible with Windows 10 at the moment. If you, dear reader, would  like to work with us on Windows 10 GUI agent and GUI domain support, please let us know!)

Currently, many parts of the Qubes architecture assume a singular target GUI domain (or audio domain) for every qube. There may be multiple GUI domains in the system, but each qube can only use one of them. We do not plan to change this in the foreseeable future.

## Plans for the future

Introducing the GUI domain opens up a lot of interesting new possibilities. First and foremost, even in the middle-of-the-road, painful-compromise solution, dom0 will still be much, much smaller (no desktop managers or huge graphical libraries), thus it can be much more easily ported to another distribution.

A smaller dom0 could also be placed completely in RAM, making the whole disk controller and storage subsystem independent from it and possibly isolated in its own storage domain, as described in the [Qubes Architecture Specification](/attachment/doc/arch-spec-0.3.pdf) only 10 years ago. Now we're finally moving closer to this goal!

Finally, decoupling support for VNC and other remote desktop capabilities opens the door to various server-based solutions in which Qubes can run on a remote server, and we can delegate some or all of our domains to other machines (potentially with faster harder and more resources). This is a another step toward [Qubes Air](/news/2018/01/22/qubes-air/).


