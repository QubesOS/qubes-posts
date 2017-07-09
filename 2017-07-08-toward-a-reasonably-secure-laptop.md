---
layout: post
title: "Toward a Reasonably Secure Laptop"
categories: announcements
author: Andrew David Wong
---

It's no secret that hardware selection is one of the biggest hurdles Qubes
users face. Finding a computer that is secure, trustworthy, and compatible
is more difficult than it should be. In an effort to address the compatibility
aspect of that problem, we [introduced the Qubes-certified laptop
program][cert-laptops] back in 2015.

So far, only one laptop has been Qubes-certified: the Purism Librem 13v1.
A number of users purchased this laptop comfortable in the knowledge that it
would be compatible with Qubes, and it served them well in that regard.
However, the Librem 13v1 is no longer being manufactured, and the Librem 13v2
has not undergone Qubes-certification (nor has any other laptop yet). This
means that the need for compatible hardware is more pressing than ever.

It's important to remember that Qubes-certification is only about
*compatibility* -- not security, trustworthiness, or anything else. Being
Qubes-certified has always meant that a computer has been tested to
ensure that it runs Qubes OS well -- nothing more, nothing less. But we know
that security-conscious users care about more than just compatibility, which
is why we announced [updated requirements for Qubes 4.x certification][cert-q4]
last year.

So far, no third-party manufacturers have produced a computer
that satisfies these requirements. However, ITL has entered initial talks with
a promising partner with whom we can foresee creating a true Reasonably Secure
Laptop. Our plan is to introduce a tier-based model of laptop support:

 - **Level 0: Qubes Compatible Laptop.** As with the Purism Librem 13v1, this
   will be a laptop that comes with no guarantees regarding security or
   trustworthiness. We'll guarantee only that the laptop is compatible with
   Qubes OS. In practice, a vendor who wishes to introduce a Level 0
   laptop will typically have to allow for specific choices regarding the GPU,
   Wi-Fi, and Bluetooth modules. The vendor will also have to be willing to
   "freeze" the configuration of the laptop for at least one year.

 - **Level 1: Qubes Certified Laptop.** In addition to meeting all the
   requirements of Level 0, this laptop will also have to conform to our
   [updated requirements for Qubes 4.x certification][cert-q4].

 - **Level 2: Qubes Stateless Laptop.** For details about this, please see
   Joanna Rutkowska's paper [State Considered Harmful][state]. We can foresee
   multiple levels of compatibility here. However, we expect that it will be at
   least two years before a true stateless laptop can be created. In the
   immediate future, therefore, we intend to pursue a Level 1 laptop.

Please note that laptops on the Qubes [Hardware Compatibility List (HCL)][hcl]
do not have a specific level. This is because neither ITL nor the Qubes OS
Project makes any affirmations regarding the vast majority of laptops on this
list. Rather, the list is compiled from voluntary contributions from members
of the community like you!

This is just the beginning. There's a long road ahead before we can make
a Reasonably Secure Laptop a reality, but the need is too great to ignore.


[cert-laptops]: /news/2015/12/09/purism-partnership/
[cert-q4]: https://www.qubes-os.org/news/2016/07/21/new-hw-certification-for-q4/
[state]: https://blog.invisiblethings.org/papers/2015/state_harmful.pdf
[hcl]: https://www.qubes-os.org/hcl/

