---
layout: post
title: "Qubes Security Team Update"
date: 2018-11-05
categories:
- announcements
- security
author: Andrew David Wong
---

As we recently announced, [Joanna Rutkowska] has turned over leadership of the
Qubes OS Project to [Marek Marczykowski-Górecki] (see [Joanna's announcement]
and [Marek's announcement]). In this post, we'll discuss the implications of
these changes for the Qubes Security Team and how we're addressing them.


What is the Qubes Security Team?
--------------------------------

The [Qubes Security Team (QST)] is the subset of the [Qubes Team] that is
responsible for ensuring the security of Qubes OS and the Qubes OS Project.
In particular, the QST is responsible for:

 - Responding to [reported security issues]
 - Evaluating whether [Xen Security Advisories (XSAs)] affect the security of
   Qubes OS
 - Writing, applying, and/or distributing security patches to fix
   vulnerabilities in Qubes OS
 - Writing, signing, and publishing [Qubes Security Bulletins (QSBs)]
 - Writing, signing, and publishing [Qubes Canaries]
 - Generating, safeguarding, and using the project's [PGP keys]

As a security-oriented operating system, the QST is fundamentally important to
Qubes, and every Qubes user implicitly trusts the members of the QST by virtue
of the actions listed above.


How does the recent change in leadership affect the QST?
--------------------------------------------------------

Until now, the two members of the QST have been Joanna and Marek. With Joanna's
new role at the Golem Project, she will no longer have time to function as a QST
member. Therefore, Joanna will officially transfer ownership of the [Qubes
Master Signing Key (QMSK)] to Marek, and she will no longer sign QSBs.

However, due to the nature of PGP keys, there is no way to guarantee that Joanna
will not retain a copy of the QMSK after transferring ownership to Marek. Since
anyone in possession of the QMSK is a potential attack vector against the
project, Joanna will continue to sign [Qubes Canaries] in perpetuity.

With Joanna's departure from the QST, Marek would remain as its sole member.
Given the critical importance of the QST to the project, however, we believe
that a single member would be insufficient. Therefore, after careful
consideration, we have selected a new member for the QST from among our
experienced Qubes Team members: [Simon Gaiser (aka HW42)][Simon].


About Simon
-----------

[Simon] has been a member of the Qubes Team for over two years and has been a
contributor to the project since 2014. He has worked on many different parts of
the Qubes codebase, including core, Xen, kernel, and GUI components. Earlier
this year, he joined Invisible Things Lab (ITL) and has been gaining experience
with other security projects. His thorough knowledge of Qubes OS, ability to
assess the severity of security vulnerabilities, and experience preparing Xen
patches make him very well-suited to the QST. Most importantly, both Joanna and
Marek trust him with the responsibilities of this important role. We are pleased
to announce Simon's new role as a QST member. Congratulations, Simon, and thank
you for working to keep Qubes secure!

[Joanna Rutkowska]: /team/#joanna-rutkowska
[Marek Marczykowski-Górecki]: /team/#marek-marczykowski-g%C3%B3recki
[Joanna's Announcement]: /news/2018/10/25/the-next-chapter/
[Marek's announcement]: /news/2018/10/25/thank-you-joanna/
[Qubes Security Team (QST)]: /security/#the-qubes-security-team
[Qubes Team]: /team/
[reported security issues]: /security/#reporting-security-issues-in-qubes-os
[Xen Security Advisories (XSAs)]: /security/xsa/
[Qubes Security Bulletins (QSBs)]: /security/bulletins/
[Qubes Canaries]: /security/canaries/
[PGP keys]: https://keys.qubes-os.org/keys/
[Qubes Master Signing Key (QMSK)]: /security/verifying-signatures/#1-get-the-qubes-master-signing-key-and-verify-its-authenticity
[Simon]: /team/#simon-gaiser-aka-hw42

