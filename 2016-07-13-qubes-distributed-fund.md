---
layout: post
title: The Qubes Project announces a decentralized bitcoin fund
author: Joanna Rutkowska
---

As part of our quest to decentralize and harden the project, we have switched
today to a multi-signature wallet for our Qubes bitcoin fund. This means that no
longer can a single person, not even myself, sign an outgoing transaction from
our new wallet. For this to happen M out of N signatures is required (we
selected N = 13, and M = 6, for the time being). The holders of the keys have
been invited from among Qubes developers and supporters from all over the world.
Some people might have more than one key, but still fewer than M.

Currently this fund is used exclusively to receive [donations], but we're
planning some more usecases for it in the near future.

We have also updated the [Qubes Security Pack] with information required to
[verify] the authenticity of the address. (We especially encourage anyone making
a larger donation to verify the address before sending bitcoin.)

Why is this important? With the Qubes OS userbase [growing] dynamically, it's
only a matter of time before more powerful actors start to see the project as a
target, with the intention to weaken it or shut it down. We want to prepare for
that moment as best we can. This involves making the project resistant to
wrongdoing by any single person and minimizing the incentive to target core
people.

Today's move to a decentralized multi-signature wallet for the Qubes bitcoin
fund is one of the many steps towards achieving this goal. Other solutions we
have already implemented include: not-trusting any of the 3rd party
infrastructure (e.g. for building packages or storing signing keys), using
easy-to-migrate-and-mirror git-based hosting (i.e. not being dependent on, and
certainly not trusting, GitHub), Qubes VM-enforced sandboxing for
template builds (to protect our developers, and consequently the official
builds), using Tor for routing all the traffic to/from the VMs used for building
the official packages and releases (to increase the difficulty of targeted
injection of backdoored 3rd party packages, e.g. from Fedora or Debian, by an
adversary who gets access to 3rd party signing keys), and offering users the
ability to easily download all updates over Tor (to increase the difficulty of
targeted attacks against our users). And we're planning
more: obligatory multi-signatures on the source code (qubes-builder should
require e.g. at least 2 signed tags before building any official package), and,
of course, multi-signed binaries and ISOs (for which we need the build process
to be reproducible).

In a few years, I hope that Qubes will become fully decentralized, with no
single person having the capacity to compromise it.

However, it is important to clarify that by "decentralization" we do not
necessarily mean "democratization." If our goal were to democratize the project,
there would be a risk that, without clear leadership, the project could stagnate
or wander aimlessly. Even worse, the project could be misdirected into a
blind avenue, whether due to lack of skill, naivety, or perhaps the intentional
actions of those among whom we decide to distribute power.

Consequently, we're not interested in decentralization of the current leadership
(at least for fundamentally important matters, such as the general roadmap and
architecture), but in building in various mechanisms to prevent any potential
corruption.

[donations]: /donate/
[Qubes Security Pack]: https://github.com/QubesOS/qubes-secpack
[verify]: /doc/security-pack/
[growing]: /counter/
