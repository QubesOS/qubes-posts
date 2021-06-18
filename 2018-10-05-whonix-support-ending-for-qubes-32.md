---
layout: post
title: "Whonix support ending for Qubes 3.2"
date: 2018-10-05
categories: announcements
author: Andrew David Wong
---

Due to limited developer time and resources, the [Whonix Project] will
end support for Qubes 3.2 on 2018-11-15.


Whonix support for Qubes OS
---------------------------

Please note that there is a distinction between Qubes supporting Whonix
and Whonix supporting Qubes.

1. Qubes supporting Whonix means that Qubes OS allows the secure
   installation and use of [Whonix TemplateVMs] inside of Qubes OS. In
   this case, the Qubes developers work to ensure that code on the Qubes
   side is set up to accommodate Whonix TemplateVMs. (This is the same
   sense in which Qubes supports Fedora and Debian TemplateVMs.) Here is
   a [table][templatevm-support] of the TemplateVM types that Qubes
   supports.

2. Whonix supporting Qubes means that Whonix is designed to be
   installable and usable as a pair of TemplateVMs inside of Qubes OS.
   In this case, the Whonix developers work to ensure that code on the
   Whonix side is set up to work inside of Qubes OS. (Similarly, the
   Whonix Project also works to ensure that Whonix can be installed
   inside of VirtualBox, for example.) Here is the [Whonix version
   support policy] for Qubes OS.

Both directions of support are necessary in order to ensure that Whonix
functions properly inside of Qubes, and the Qubes and Whonix developers
work together toward this shared goal.

This particular announcement concerns the second direction of support:
Whonix supporting Qubes (in particular, ending support for Qubes 3.2).


Difference from EOL
-------------------

[Whonix 13 recently reached EOL (end-of-life) on
2018-09-30][whonix-13-eol]. When an OS or TemplateVM version reaches
EOL, it no longer receives support from its maintainer. In this
announcement, however, nothing is reaching EOL. Whonix is ending support
for Qubes 3.2 2018-11-15, but the Qubes OS Project will continue to
support Qubes 3.2 as planned until [2019-03-28][qubes-3.2-eol].


What this means for you as a user
---------------------------------

If you are using Qubes 4.0, this announcement does not affect you. If
you are using Qubes 3.2, the Whonix Project will no longer support your
system after 2018-11-15. This means that no developers from the Whonix
Project will be monitoring or working on issues that pertain solely to
Qubes 3.2. Therefore, the Whonix Project cannot guarantee that Whonix
will continue to function as expected on Qubes 3.2.

However, since Qubes 3.2 is a mature platform, it is likely that Whonix
will continue to work normally until Qubes 3.2 reaches EOL on
2019-03-28. Users who decide to continue using Whonix on Qubes 3.2 do so
at their own risk. It is possible that an upgrade could break certain
functionality, such as apt-get upgrading, networking, VM booting, or VM
graphics. The Whonix Project believes it is unlikely (though not
impossible) that a clearnet leak would result from continued use. For
further assistance, please consult the [Whonix support page].


[Whonix Project]: https://www.whonix.org/
[Whonix TemplateVMs]: /doc/whonix/
[templatevm-support]: /doc/supported-versions/#templates
[whonix-13-eol]: /news/2018/08/24/whonix-13-approaching-eol/
[Whonix version support policy]: /doc/supported-versions/#note-on-whonix-support
[qubes-3.2-eol]: /news/2018/03/28/qubes-40/#the-past-and-the-future
[Whonix support page]: https://www.whonix.org/wiki/Support

