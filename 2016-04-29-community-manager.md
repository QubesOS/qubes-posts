---
layout: post
title: Joining the Qubes Team as Community Manager
date: 2016-04-29
author: Andrew David Wong
categories:
  - announcements
---

<style>
  pre.sig {
    border: 0;
    padding-left: 0;
    background-color: #fff;
  }
</style>

<pre class="sig">
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512
</pre>

I'm pleased to announce that I've joined the Qubes team in a part-time
role as Community Manager. I consider it an honor to have the
opportunity to work with such a talented team of individuals and to serve
such a dynamic community. As the Community Manager, I'll primarily be
responsible for things like handling user feedback, organizing bug
reports, tracking community-developed features, and facilitating community
contributions to the codebase. (As with any small project, however, we all
wear many hats, so if there's ever anything Qubes-related I can help you
with, please let me know.) 

I've been active in the Qubes community for several years now under the
pseudonym "Axon," primarily writing documentation and helping to maintain
the Qubes website in my spare time as a volunteer (which I plan to continue
doing alongside my new role).  In joining the Qubes team more
officially, however, I've decided to retire my pseudonym and to begin
using my real identity. I consider myself fortunate to be in a position
to make this choice. For me, the decision to use a pseudonym was based
primarily on considerations of personal privacy. For many other people
around the world, however, pseudonymous and anonymous communication is a
matter of life and death. This is one reason that I believe Qubes OS
-- and especially its partnership with Whonix -- is so important:
it allows for the secure compartmentalization of these various contextual
identities (along with all the other areas of one's digital life) in ways
which would not otherwise be possible. More importantly, however, it
freely puts this control in the hands of individual users.

Admittedly, it currently takes a certain kind of user -- one who is
sufficiently self-motivated and willing to learn -- to make full use of
Qubes OS. This is something we're continually working on. By working to
make Qubes accessible to a wider user base, we aim to make strong
endpoint security available to everyone, regardless of their level of
technical expertise. As computers continue to become increasingly
integrated with our lives (and our bodies), the importance of secure
computing increases proportionately for all of us.


## Identity Verification ##

If you'd like to verify my identity, I'd be happy to help. There are
three PGP keys you may wish to authenticate:

                  Key                                     Fingerprint                   
    ===============================   ==================================================
    Andrew David Wong (primary)       BBAF 910D 1BC9 DDF4 1043  629F BC21 1FCE E9C5 4C53
    Axon (retired pseudonym)          746A B6DE 2A02 B5A5 DCBD  3F32 A4EC AE9C 8E97 231E
    Qubes Documentation Signing Key   E11D 15C6 D204 3576 9FFA  A456 8CE1 3735 2A01 9A17

You can fetch these keys from my [website][]:

    $ gpg --fetch \
    https://andrewdavidwong.com/adw.asc \
    https://andrewdavidwong.com/axon.asc \
    https://andrewdavidwong.com/qubes-doc.asc

Or import them from my [keys][] repo:

    $ git clone git://github.com/andrewdavidwong/keys
    $ gpg --import keys/*

Or retrieve them from a public key server:

    $ gpg --recv \
    0xBBAF910D1BC9DDF41043629FBC211FCEE9C54C53 \
    0x746AB6DE2A02B5A5DCBD3F32A4ECAE9C8E97231E \
    0xE11D15C6D20435769FFAA4568CE137352A019A17

Once you have all three keys, you can check that they've all been
level-3 certified by one another:

    $ gpg --check-sigs \
    0xBBAF910D1BC9DDF41043629FBC211FCEE9C54C53 \
    0x746AB6DE2A02B5A5DCBD3F32A4ECAE9C8E97231E \
    0xE11D15C6D20435769FFAA4568CE137352A019A17

Then, you can verify that this post itself has been signed by all three
keys. There are several ways to do this. Since this post is a clearsigned
message block, the most obvious way is simply to copy it directly from
your browser. Start by telling GPG that you'd like to verify something:

    $ gpg --verify

Next, copy the signed message block from this page to your clipboard.
(Extraneous text is fine, so you can simply press `ctrl + A` to select
all the text on this page, followed by `ctrl + C`.) Then, simply paste
it into your terminal emulator and press `ctrl + D`. GPG will proceed
to verify the signatures.

Alternatively, you may prefer a [clearsigned, plain text version][clear]
of this post. (Perhaps your browser has rendered the text differently,
invalidating the signature, or you simply don't want to bother with all
that copying and pasting.) After downloading and reviewing the file,
you can verify it like so:

    $ gpg --verify _2016-04-29-community-manager.asc

Or download and verify in a single command:

    $ curl https://raw.githubusercontent.com/QubesOS/qubes-posts/master/_2016-04-29-community-manager.asc | gpg --verify

Another alternative is to verify the [original source file of this
post][source] against its [detached signature][sig]. You can
download each file, then verify:

    $ gpg --verify _2016-04-29-community-manager.md.sig 2016-04-29-community-manager.md

Or, if you're feeling adventurous, do it all in a single command:

    $ gpg --verify \
    <(curl https://raw.githubusercontent.com/QubesOS/qubes-posts/master/_2016-04-29-community-manager.md.sig) \
    <(curl https://raw.githubusercontent.com/QubesOS/qubes-posts/master/2016-04-29-community-manager.md)

Then, you can verify the three tags on the commit for this post --
each one singed by its respective key:

    $ git clone git://github.com/QubesOS/qubes-posts.git
    $ cd qubes-posts/
    $ git verify-tag adw
    $ git verify-tag axon
    $ git verify-tag qubes-doc-adw

Finally, you can verify my Qubes Documentation Signing Key signature on
the commit itself:

    $ git verify-commit `git rev-list -n 1 adw`

With any luck, you're now convinced that all three keys belong to the same
person and that this person is the author of the words you're reading.
For other objects signed by these keys, feel free to check out my signed
commits to the Qubes [website][web-repo] and [documentation][doc-repo]
repos, my signed emails to the [mailing lists], and the proofs available
from my [Keybase][] profile.

You're also welcome to contact me privately (e.g., to request key
authentication through alternative channels). For personal correspondence,
`adw@andrewdavidwong.com` functions as both an [email][adw-email] address
and an [XMPP][adw-xmpp] address. My [OTR][] key fingerprint is:

    1A5F4647 4CEEA362 4F740943 D4F40D4F CC1116CB

Please direct all Qubes-specific correspondence to [adw@qubes-os.org][].

Finally, please note that since I'm no longer using the "Axon" pseudonym,
I'll begin using my primary key (`0xBC211FCEE9C54C53`) instead of the
Axon key (`0xA4ECAE9C8E97231E`) for everything except Qubes documentation
signing. I intend to allow the latter's active subkeys to expire without
renewal on 2016-10-03.

Thanks for reading!

[website]: https://andrewdavidwong.com/
[keys]: https://github.com/andrewdavidwong/keys
[clear]: https://raw.githubusercontent.com/QubesOS/qubes-posts/master/_2016-04-29-community-manager.asc
[source]: https://raw.githubusercontent.com/QubesOS/qubes-posts/master/2016-04-29-community-manager.md
[sig]: https://raw.githubusercontent.com/QubesOS/qubes-posts/master/_2016-04-29-community-manager.md.sig
[web-repo]: https://github.com/QubesOS/qubesos.github.io
[doc-repo]: https://github.com/QubesOS/qubes-doc
[mailing lists]: /mailing-lists/
[Keybase]: https://keybase.io/adw
[adw-email]: mailto:adw@andrewdavidwong.com
[adw-xmpp]: xmpp:adw@andrewdavidwong.com
[OTR]: https://en.wikipedia.org/wiki/Off-the-Record_Messaging
[adw@qubes-os.org]: mailto:adw@qubes-os.org

<pre class="sig">
-----BEGIN PGP SIGNATURE-----

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
=4P8G
-----END PGP SIGNATURE-----
</pre>

