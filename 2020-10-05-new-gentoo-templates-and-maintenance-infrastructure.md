---
layout: post
title: "New Gentoo templates and maintenance infrastructure"
categories: articles
author: Frédéric Pierret, Marek Marczykowski-Górecki
---

_This is the third article in the "What's new in Qubes 4.1?" series.
Previously: [The New Qrexec Policy
System](https://www.qubes-os.org/news/2020/06/22/new-qrexec-policy-system/)
and
[The GUI Domain](https://www.qubes-os.org/news/2020/03/18/gui-domain/)._

New Gentoo templates
--------------------

The work I've been doing on Gentoo templates is finally ready to be
released! The corresponding issue is
[#4412](https://github.com/QubesOS/qubes-issues/issues/4412), where you
can find almost every related piece of work. I would like to highlight
that this has been a great opportunity to collaborate with the Gentoo
core team, and multiple improvements have been implemented on the Gentoo
side thanks to the help of Gentoo devs **mgorny** and **zmedico**. When
I encountered issues, I appreciated the quick response from several
maintainers of different Gentoo components.

Why a Gentoo template? Gentoo is designed for users who want to
customize a Linux distribution to fit their specific needs. The benefit
in Qubes is that it allows one to create highly customized and hardened
TemplateVMs (or StandaloneVMs). For example, one could customize the
Gentoo Qubes Builder to create a [ClipOS](https://clip-os.org/en/) build.

The new Gentoo templates are available in tree flavors. The [default
(Gnome)](https://www.qubes-os.org/doc/templates/gentoo/),
[minimal](https://www.qubes-os.org/doc/templates/minimal/), and
[XFCE](https://www.qubes-os.org/doc/templates/xfce/). Currently, they
are available in the `qubes-templates-community-testing` repo, and
they'll soon be in the `qubes-templates-community` repo.

Maintenance infrastructure
--------------------------

In order to keep the new Gentoo template in good working condition, we
need a set of automated tests. The bare minimum is continually testing
whether just building the template still works. Due to the nature of
Gentoo, such tests require far more resources (time, CPU power) than are
available for open-source projects on Travis CI (which we use for
testing other templates). We use Travis CI as a bare minimum for
validating every pull request, then it generally goes to openQA. Here,
the issue with Gentoo is that, by definition, it's a source
distribution, so everything needs to be rebuilt. Travis allows a maximum
timeout of something like 50 minutes for jobs. Simply rebuilding a Qubes
component for Gentoo takes several hours (and more than half a day for
each template). So, Travis is out.

I've set up in our pull request pipeline the use of a feature of Gentoo
that allows us to get pre-built binaries from a mirror. For that, when I
build a full template, I push a fresh repository with every package
built on my mirror. But still, even when doing this, hours are needed
for jobs. I didn't want to give up on this, so, after evaluating several
options, I decided to set up my own self-hosted GitLab CI instance. For
that, I've developed a service that I call
[qubes-g2g-continuous-integration](https://github.com/fepitre/qubes-g2g-continuous-integration/),
forwarding selected GitHub pull requests to my GitLab CI instance.

In consequence, I can manage the allocated resources for Gentoo builds,
and we now have the means to validate every pull request for each
component that has been integrated into Gentoo. A check status appears
on GitHub side-by-side with Travis checks for other distros where
everyone can access the build logs too. Here is an
[example](https://gitlab.notset.fr/fepitre-bot/qubes-app-linux-input-proxy/-/pipelines/346).
By the way, this is also what we use for automatically checking kernel
pull requests. Here is a [recent pull
request](https://github.com/QubesOS/qubes-linux-kernel/pull/276). When
viewing the checks, you can see the results for the kernel builds. Once
again, it's because the build time is superior to what Travis allows.

Conclusion
----------

All this infrastructure is intended not only for the kernel and Gentoo,
but also to help Arch Linux users. Depending on the needed resources, we
could also add the longer Arch Linux jobs into my GitLab CI instance,
because we currently don't properly validate the template itself.

In general, this new infrastructure allows us to create and test
pipelines that require a lot of resources. Combined with Travis and
openQA, we have another layer to rely on for validating and automating
the building of cutting edge templates like Gentoo and Arch Linux. While
the same kinds of features are available on paid GitLab plans, I
preferred to do it myself with a free self-hosted instance, which allows
me to provide as many workers as I have available.

Our overarching goal here is to broaden the scope of things in Qubes
that we test automatically. This helps not only with increasing the
quality of software we deliver, but also with saving developers' time,
since more automated testing means less need for time-consuming manual
testing.

