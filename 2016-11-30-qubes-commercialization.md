---
layout: post
title: Qubes OS Begins Commercialization and Community Funding Efforts
author: Joanna Rutkowska
---

Since the initial [launch][alpha] of Qubes OS back in April 2010, work on Qubes
has been funded in several different ways.  Originally a pet project, it was
first supported by [Invisible Things Lab][itl] (ITL) out of the money we earned
on various R&D and consulting contracts. Later, we decided that we should try to
commercialize it. Our idea, back then, was to commercialize Windows AppVM
support.  Unlike the rest of Qubes OS, which is licensed under GPLv2, we thought
we would offer Windows AppVM support under a proprietary license. Even though we
made a lot of progress on both the business and technical sides of this
endeavor, it ultimately failed.

Luckily, we got a helping hand from the [Open Technology Fund][otf] (OTF), which
has [supported][otf-qubes] the project for the past two years. While not a large
sum of money in itself, it did help us a lot, especially with all the work
necessary to improve Qubes' user interface, documentation, and outreach to new
communities.  Indeed, the (estimated) Qubes user base [has grown][counter]
significantly over that period. Thank you, OTF!

But Qubes is more than just a nice UI: it's an entirely new, complex system ---
a system that aims to change the game of endpoint security.  Consequently, it
requires expertise covering a wide spectrum of topics: from understanding
low-level aspects of hardware and firmware (and how they translate to the
security of a desktop system), to UI design, documentation writing, and
community outreach. Even if we consider only the "security research" aspect of
Qubes, this area alone easily scales beyond the capabilities of a single human
being.

In order to continue to deliver on its promise of strong desktop security, Qubes
must retain and expand its core team, and this requires substantial funding. At
this point, we believe the only realistic way to achieve this is through
commercialization, supplemented by community funding.

## Commercialization ##

We're taking a different approach to commercialization this time.  Building on
the success of the recent Qubes 3.2 release, which has been praised by users for
its stability and overall usability, we will begin offering commercial editions
(licenses) of Qubes OS to corporate customers. We believe that the maturity of
Qubes, combined with its powerful new [management stack][salt], makes it ripe
for adoption by any corporation with significant security needs.

Commercial editions of Qubes OS will be customized to meet special corporate
requirements. For example, two features that might be particularly attractive to
corporate customers are (1) "locking down" dom0 in order to separate the user
and administrator roles and (2) integrating our local management stack with a
corporation's remote management infrastructure. These are both examples of
features that our developers are capable of implementing now, on Qubes 3.2.

We plan to partner with one to three corporate clients in order to run a pilot
program throughout the first half of 2017.  After it has been successfully
completed, we'll then widen our offer to more corporate customers and,
ultimately, to small business customers. Our main constraint is the scalability
required to cover each additional client. Hence, we plan to focus on larger
customers first.

Let there be no misunderstanding: Qubes OS will always remain open source. We
anticipate that the majority of our commercialization efforts will involve the
creation of custom Salt configurations, and perhaps writing a few additional
apps and integration code. In the event that any corporate features require
reworking the core Qubes code, that new code will remain open source.

We considered many other ways of attempting to commercialize Qubes before
arriving at this model. One possibility that some of our users have inquired
about is that we sell dedicated Qubes hardware (i.e. laptops). However, there
are a number of challenges here, both in terms of making the hardware
trustworthy enough to merit our "seal of approval", and from a business and
logistics perspective. For these reasons, we don't plan to pursue this option in
the immediate future.


## Community funding ##

Unfortunately, the financial necessity of shifting our priorities to commercial
clients will mean that we have less time to work on features that benefit the
wider, security-minded open source community, which has been our focus for the
past seven years.  This deeply saddens us. (We all use Qubes on our personal
computers too!) However, the reality is that ITL can't afford to sustain the
open source development of Qubes for much longer. We're running out of time.

In an attempt to keep the open source development of Qubes going, we've teamed
up with [Open Collective][oc], which makes it easier to donate to the Qubes
project.  Now, in addition to our [Bitcoin fund][btc], we can also accept
donations via credit card. ITL will not benefit from any of the money donated
through Open Collective. Instead, the funds will be paid directly to individual
developers who have been hired to work on the open source edition of Qubes.
With the help of our community, we hope eventually to build a nonprofit
organization that will ensure the long-term future of Qubes as an open source
operating system that is freely available to all --- one of the few operating
systems that places the security of its users above all else.

If you are a user of Qubes and want to help us continue working on it, please
[donate now][oc].  Those who have contributed will be publicly recognized on our
[Open Collective][oc] page (if they so choose).  Organizations that support the
Qubes project will be publicly recognized on our [Partners page][partners]
(again, if they so choose).  If you are interested in supporting Qubes with
significant resources, whether as an individual or on behalf of an organization,
we ask that you please [contact us directly][business], since donating through
Open Collective entails significant administrative overhead.

Thank you for your continued support. Together, we can ensure that Qubes is
around to secure our digital lives for many years to come.

--- [The Qubes team][team]

--------------------------------------------------------------------------------

## Q & A ##

(Added 2016-12-05)

Here are some of the questions we've received from the community regarding this
announcement.

**Q:** I'm concerned that large donors might weild undue influence over the
direction of the Qubes project. Is this something users should worry about?

**A:** No. Our position has always been --- and continues to be --- that donors
do not have any special influence over the direction of the project. Making a
donation does not entitle anyone to decide or vote on development priorities.

**Q:** Will the process of receiving and spending donations be transparent?

**A:** Yes. All donations received through Open Collective will be publicly
visible, as will the way this money is spent. (You can view this on our [Open
Collective page][oc].)

**Q:** Can you give me an idea of how much money the project needs?

**A:** I think some kind of measurement is that we've received $410k from OTF
over the last year. During that time, we've got also received some much smaller 
donations, but that doesn't change the above figure much. 
This was enough to survive and release new versions, but not enough to 
implement everything we've planned for. Some examples:

 - Qubes 4.0 is delayed more and more, 
 - We haven't managed to add Gnome support 
 - Live USB is still in alpha phase and basically unmaintained 
 - A lot of bugs are not fixed (see GitHub issues) 

This is a lot of money. And we don't think it's realistic to collect it 
just from public donations. This is why we're introducing a commercial 
edition. But that doesn't mean we don't need your support. On the 
contrary! We'd love to continue work on the open source edition as much as 
possible! Not only as a base for commercial product, but also as a fully 
usable and functional system on its own!

**Q:** The announcement says that if I'm interested in supporting Qubes with
"significant resources," I should contact you directly. How much is
"significant"?

**A:** I think anything >= $10k. We also have some internal overhead of 
handling individual donations manually (mostly related to the number of 
such donations, not necessarily the amount), so it's not really worth it for
smaller amounts. That's why we have Open Collective to handle these.

**Q:** Will any/all of the current ITL devs continue to work on the open source
edition of Qubes?

**A:** Probably, but it depends on how much of their time is demanded by
corporate clients. (Remember, though, that any changes to core code will remain
open source, so it's quite likely that the core devs will continue to contribute
to the open source edition, at least indirectly.)

**Q:** Will any/all of the current non-ITL devs continue to work on the open
source edition of Qubes?

**A:** Probably, but it depends on what they want to do, how much gets donated,
and therefore how much money is available to pay them. 

**Q:** If new developers are hired, will the quality of their code be reviewed
to ensure the high standards of the Qubes project are maintained?

**A:** Yes, absolutely.


[alpha]: https://blog.invisiblethings.org/2010/04/07/introducing-qubes-os.html
[itl]: https://invisiblethingslab.com
[wni]: https://blog.invisiblethings.org/2014/01/15/shattering-myths-of-windows-security.html
[otf]: https://www.opentech.fund/
[otf-qubes]: https://www.opentech.fund/project/qubes-os
[counter]: https://www.qubes-os.org/counter/
[donations]: https://www.qubes-os.org/donate/
[oc]: https://opencollective.com/qubes-os
[salt]: https://www.qubes-os.org/news/2015/12/14/mgmt-stack/
[btc]: https://www.qubes-os.org/news/2016/07/13/qubes-distributed-fund/
[partners]: https://www.qubes-os.org/partners/
[business]: mailto:business@qubes-os.org
[team]: https://www.qubes-os.org/team/
