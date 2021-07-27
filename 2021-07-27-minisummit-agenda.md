---
layout: post
title: "Upcomming Qubes OS - 3mdeb mini-summit 2021"
categories: announcements
author: Marek Marczykowski-Górecki
---

We are pleased to announce agenda for the upcoming Qubes OS - 3mdeb
mini-summit. The event will take place in two sessions on August 3rd and 10th.
Each day, there will be 4 talks, intermixed with Q&A time.

The agenda for August 3rd (all times UTC):

- 18:00 - 18:15 - welcome/introduction (Piotr Król)
- 18:15 - 19:00 - Qubes OS 4.1 highlights (Marek Marczykowski-Górecki)
- 19:00 - 19:45 - First Impressions Count: Onboarding Qubes Users Through an Integrated Tutorial (deeplow)
- 19:45 - 20:15 - Break
- 20:15 - 21:00 - Wyng-backups - revertible local and remote known safe QubesOS states (including dom0) (Thierry Laurion)
- 21:00 - 21:45 - SRTM and Secure Boot for VMs (Piotr Król)
- 21:45 - vPub, informal afterparty

The agenda for August 10th (all times UTC):

- 18:00 - 18:15 - welcome/introduction (Piotr Król)
- 18:15 - 19:00 - Usability Within A Reasonably Secure, Multi-Environment System (Nina Alter)
- 19:00 - 19:45 - Qubes OS Native App Menu: UX Design and Implementation (Marta Marczykowska-Górecka, Nina Alter)
- 19:45 - 20:15 - Break
- 20:15 - 21:00 - Brief history of USB camera support in Qubes OS (Piotr Król)
- 21:00 - 21:45 - How to setup BTC and XMR cold storage in Qubes OS (Piotr Król)
- 21:45 - vPub, informal afterparty

## Abstracts of the talks

### Qubes OS 4.1 highlights

Upcoming Qubes OS 4.1 release is full of new exciting features, ranging from a
technology preview of GUI domain, to subtle, yet important, Qrexec
improvements. In this talk I will give a brief overview of them and demo a
select few.

### First Impressions Count: Onboarding Qubes Users Through an Integrated Tutorial

We may all relate to having a rough time when starting using Qubes -- be that
because we're coming from Windows and everything is different or because we
come from Linux and many things don't work like we expect them to. Apart from
the usual challenges of going into a different system, Qubes has the additional
one of requires a fundamentally different way to think about your computer (a
hypervisor mental-model). Smoothing out this transition is particularly
important as Qubes aims to target also vulnerable populations, that are less
technically inclined and with less time to explore and read the documentation.

The solution proposed by [deeplow](https://www.qubes-os.org/team/#deeplow) is
to implement an integrated onboarding tutorial. The idea is that a short
tutorial (with optional extra ones) that guides the user through the essential
mechanics of Qubes will make the transition simpler. That's what deeplow's been
working on for his master's dissertation. In this talk he'll introduce the idea
and give an update on the current progress and challenges.

### Wyng-backups - revertible local and remote known safe QubesOS states (including dom0)

[Wyng-backups](https://github.com/tasket/wyng-backup) is an incremental
backup/restore tool for LVMs. For QubesOS, this means even dom0 can be reverted
to a known safe state; locally or remotely, applying changes only. Deep dive
into possibilities of wyng-backups to deploy and maintain up to date,
revertible states of QubesOS base systems.

### SRTM and Secure Boot for VMs

The talk is the continuation of the Qubes OS mini-summit presentation
"SRTM for Qubes OS VMs", where the theoretical background o Static Root
of Trust was presented and discussed. In this presentation, we would
like to practically approach SRTM and Secure Boot for VMs. We will also
explore potential use cases for self-decrypting storage and signed
kernel using safeboot. Finally, we will discuss how to introduce this
and other security mechanisms in Qubes OS.

### Usability Within A Reasonably Secure, Multi-Environment System

The talk is the continuation of the Qubes OS mini-summit presentation
"SRTM for Qubes OS VMs", where the theoretical background o Static Root
of Trust was presented and discussed. In this presentation, we would
like to practically approach SRTM and Secure Boot for VMs. We will also
explore potential use cases for self-decrypting storage and signed
kernel using safeboot. Finally, we will discuss how to introduce this
and other security mechanisms in Qubes OS.

### Qubes OS Native App Menu: UX Design and Implementation

A brief overview of the new Application Menu that's being introduced in (at
latest) Qubes 4.2, the process of creating it, and design and
implementation challenges. Based on design work by Nina Alter and
implementation by Marta Marczykowska-Górecka.

### Brief history of USB camera support in Qubes OS

The use of complex multi-endpoint isochronous USB devices in the
presence of sys-usb was not always possible in Qubes OS. Luckily, the
Qubes Video Companion project was created by Elliot Killick, which
enabled users to use USB cameras on Qubes OS. The project is still in
development and testing, but it is very promising and gives many USB
cameras users hope. This presentation would like to tell a story of
using Qubes Video Companion for enabling Logitech C922.

### How to setup BTC and XMR cold storage in Qubes OS

Cold storage, also called offline wallets, provides the highest level of
security for cryptocurrency. The critical characteristic of a computing
environment that can be used as cold storage is the lack of network
connectivity. Good examples of cold storage are spare computer devices
or microcontroller-based devices like a hardware wallet. By leveraging
the same architecture, Qubes OS domains can be used for cold storage. In
such a case, one of the domains is disconnected from the network and
runs a cryptocurrency wallet inside. Other domains may generate
transactions files, which are sent to cold storage VM for signing.
Signed transaction files are sent back to the online environment. All
operations are performed using well-specified and secure Qubes RPC. This
presentation will show how to set up and use BTC and XMR cold storage
with the most popular wallets for those cryptocurrencies. We will also
discuss what other measures can be taken to secure offline wallet VM.

## Summary

The discussion section will be live meeting on Jitsi. The whole event will be
also streamed to YouTube on 3mdeb's channel, where we will also accept
questions.

We invite everyone interested to join!
