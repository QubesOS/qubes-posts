---
layout: post
title: "Qubes Architecture Next Steps: Preloaded diposables"
categories: articles
author: Benjamin Grande
---

<!--
TODO: reviewer: when posting, rename this file with correct date.
TODO: reviewer: when posting, rewrite the paragraph below:

This is the second article in the "What's new in Qubes 4.3?" series. You
can find the previous one (about GUI Domains)
[here](/news/2020/03/18/gui-domain/). While the
introduction of GUI domains is a big, singular feature, the changes to
qrexec are more complex and varied --- but also very important.
-->

## What are disposables?

A [disposable](/doc/how-to-use-disposables/) is a lightweight qube that can be
created quickly and will self-destruct when closed. Disposables are usually
created in order to host and execute untrusted code, be it on the software
level of single application (like a viewer, editor or web browser) or the
hardware level (e.g., for [PCI passtrough](/doc/how-to-use-pci-devices/)).

There are two kinds of disposables, unnamed and named. The difference between
them (besides one having a fixed name) is that unnamed disposables are
destroyed when closing the first application opened in them while the user
must explicitly shut down named disposables.

## Fresh disposable startup time leads to fatigue

One of the most secure ways to segregate duties is to use a fresh disposable
for every new task. Unnamed disposables are ideal for this use case. So, what
is the problem with them? The caller has to wait for a complete qube startup
before running the desired application. The delay might seem a minor annoyance
at first, but over a prolonged period, fatigued users tend to reuse already
tainted disposables or run applications in non-disposable qubes to avoid the
waiting time.

The problem is not the user's lack of understanding or a lack of documentation
but how the user perceives the system. If the system is slow, many users will
circumvent that slowness even if it means compromising their security, because
they just want to get things done.

The slowdown is aggravated when requesting a high number of disposables
sequentially, which happens constantly when using the Qubes Executor to
[build QubesOS](/doc/qubes-builder-v2/).

A secure workflow should not be a burden. Can Qubes OS do better?

## What are preloaded disposables?

Yes! It can do better.

Qubes OS project lead Marek Marczykowski-Górecki, in his
[Qubes OS development status update](https://cfp.3mdeb.com/qubes-os-summit-2024/talk/AWCBJ8/)
at Qubes OS Summit 2024, mentioned plans for faster disposables in Qubes OS
4.3. Here is where preloaded disposables enters.

Preloaded disposables are unnamed disposables started in the background and
kept hidden from the user when not in use. They are interrupted (paused or
suspended, as appropriate) and resumed (transparently) when a disposable qube
is requested by the user.

When the qube is preloaded, the qube application listing and the qube entry
itself are hidden from GUI applications such as the app menu and the Qrexec
Policy Ask prompt. This is by design to avoid contamination. A preload is not
something intended to be used, but indirectly.

The use of preloaded disposables is transparent, indistinguishable from the
use of unnamed disposables. Requesting the creation of a new unnamed
disposable will instead mark a preload as used and reply with an
already-running preloaded disposable, followed by the creation of a
substitute. A preload that is marked as used ceases to be a preload. Its
applications entries become visible in GUI applications the same way as
standard unnamed disposables.

Usage of disposables that have completed the preloading stage is almost as
fast as executing code on an already-running qube, just a bit slower because
it has to unpause the qube as well as start the GUI agent. While unpause can
take just a few milliseconds, waiting for the GUI agent can take a second. The
remaining time is overhead of operations across [domains](/doc/qrexec/).

In case of a failure to preload for any reason, such as lack of available
memory or the daemon responsible for managing qubes (`qubesd`) having stopped,
no user interaction is needed. The system will fill in gaps of missing
preloaded disposables while removing incomplete ones.

## How to use preloaded disposables

Preloading is a "set and forget" operation. Configure it once, setting the
desired maximum number of preloaded qubes you would like to have preloaded,
and the system takes care of the rest, including filling in the gaps of used
preloaded qubes.

The only action required to preload a disposable is to set the feature
`preload-dispvm-max` on the disposable template you most use for generating
unnamed disposables. Consider a user that does a lot of management operations
with `qvm-console-dispvm`. The default qube for those operations is
`default-mgmt-dvm`:

[![VM Settings](/attachment/posts/preload-local.png)](/attachment/posts/preload-local.png)

Or use the equivalent command-line operation:

```shell
qvm-features default-mgmt-dvm preload-dispvm-max 2
```

If you use the global `default_dispvm` a lot, you can target the global
preload setting by setting the feature on `dom0`:

[![Global settings](/attachment/posts/preload-global.png)](/attachment/posts/preload-global.png)

Or use the equivalent command-line operation:

```shell
qvm-features dom0 preload-dispvm-max 2
```

While the disposable template remains the global `default_dispvm`, it will
respect the global feature and ignore the per-qube setting.

To remove all the preloaded qubes, set the feature value to `0`, `''` (empty)
or delete the feature by unchecking the box. If the feature is set on `dom0`,
it's better to delete the feature so that the preload setting to be read from
the disposable template itself and not capped to `0`.

```shell
qvm-features --delete dom0 preload-dispvm-max
```

The limit on the number of preloaded disposables you can configure depends on
how much your system can handle. If there is not enough available memory to
preload a disposable, it is skipped until another distinct event triggers it.
However, GUI applications that allow preload configuration have a hard limit
of `50`, which should be more than enough for most use cases.

The second spin button refers to the amount of free memory that you don't want
preloaded disposables to use. Setting a value of 1000 MiB won't preload
disposables if your system only has that amount of free memory at that time.
At a later time, it will retry and preload gap will be filled if there is
enough memory. On a fresh install, this value is already set according to a
percentage of your system's total memory. A value over 1000 MiB is
recommended.

## Performance improvements

<!--
TODO: ben: graphs here would be nice, but percentage would also be ok I'd say.
-->

## Known issues

Preloaded disposables are paused when they finish preloading and haven't been
requested for use yet. Paused qubes still consume allocated memory, and this
stale state does not allow for [memory redistribution](/doc/qmemman/).

Preloading multiple disposables works even on systems with as low as 8 GB of
RAM, the experience however, may not be the best when there's little available
memory. Users with machines that constantly have a lot of unused memory won't
notice a difference.
