---
layout: post
title: "Getting pristine environments faster: Preloaded diposables"
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

## TL;DR

Disposables are pristine environments. Preloaded disposables reduces the
startup latency when requesting a disposable by pre-starting the disposable
before you request it. If you just want to learn how to use it, head to the
[usage section](#how-to-use-preloaded-disposables).

## What are disposables?

A [disposable](/doc/how-to-use-disposables/) is a stateless qube, it does not
save data for the next boot. These qubes can serve various uses cases that
require a pristine environment:

- View untrusted files without other trusted files on the same system;
- Visit untrusted websites in a web browser without previous authentication
  cookies present;
- Sanitize untrusted PDFs or images and retrieve a safe-to-view image;
- Connect untrusted devices without other trusted devices or files on the same
  system;
- Fresh environment build systems (for technical users).

Disposables can be launched either directly from GUI domain's app menu or
terminal window, or from within app qubes. Disposables are generated with
names like `disp1234`, where `1234` is a random number.

There are two kinds of disposables, unnamed and named. The difference between
them (besides one having a fixed name) is that unnamed disposables are
destroyed when closing the first application opened in them (on most
workflows) while the user must explicitly shut down named disposables.

The downside of pristine environments is cold start latency, which is the time
it takes for the server to respond the request made by the client, when it
needs to start something, in this case, a disposable.

Fast response times worries every user and service provider, the provider
being your computer or hyperscalers. Various projects uses different methods
to run code on an isolated environment, sometimes to run untrusted code, other
times to access private data securely. Some implementations allow reducing
cold start:

- Amazon Web Services (AWS): Lambda with Firecracker MicroVMs on KVM
- Google Cloud Platform (GCP): Cloud Run and Functions on Containers
- Cloudflare: Workers on Google's V8 JavaScript engine

But the hyperscalers computing requirements are very different than Qubes OS.
None of the above are as isolated as a Xen domain is, they aren't intended for
long running sessions, and are not destined to desktop users which requires a
full OS-runtime.

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

Qubes OS project lead Marek Marczykowski-Górecki, in his
[Qubes OS development status update](https://cfp.3mdeb.com/qubes-os-summit-2024/talk/AWCBJ8/)
presentation at Qubes OS Summit 2024, mentioned plans for faster disposables
in Qubes OS 4.3. One year later, the author of this post, in his
[Fast and fresh disposable qubes](https://cfp.3mdeb.com/qubes-os-summit-2025/talk/FCJLF7/)
presentation at Qubes OS Summit 2025, provided a technical overview of how
that was achieved. Here is where preloaded disposables enters.

Preloaded disposables are unnamed disposables started in the background and
kept hidden from the user when not in use. They are interrupted (paused or
suspended, as appropriate) and resumed (transparently) when a disposable qube
is requested by the user.

When the qube is preloaded, the qube application listing and the qube entry
itself are hidden from GUI applications such as the app menu and the Qrexec
Policy Ask prompt. This is by design to avoid contamination. A preload is not
something intended to be used by finding them on the system and clicking to
run applications on them, but indirectly, as will be explained in the next
paragraph. For example, the disposable is classified as an internal qube, an
is treated as such in the QUI Domains Widget, with limited functionality at
this stage, to purposefully not allow running applications on it before it is
ready to be used.

The use of preloaded disposables is transparent, indistinguishable from the
use of unnamed disposables. Requesting the creation of a new unnamed
disposable will instead mark a preload as used and reply with an
already-running preloaded disposable, followed by the creation of a
substitute. A preload that is marked as used ceases to be a preload. Its
applications entries become visible in GUI applications the same way as
standard unnamed disposables. At this stage, you may notice that QUI Domains
Widget shows the disposable as any other, it has the action button to open the
file manager.

Usage of disposables that have completed the preloading stage is almost as
fast as executing code on an already-running qube, just a bit slower because
it has to start GUI agent (if requested), which can take more than a second.
The remaining time is overhead of operations across [domains](/doc/qrexec/).

In case of a failure to preload for any reason, such as lack of available
memory or the daemon responsible for managing qubes (`qubesd`) having stopped,
no user interaction is needed. The system will fill in gaps of missing
preloaded disposables while removing incomplete ones.

## How to use preloaded disposables

Preloading multiple disposables works even on systems with as low as 8 GB of
RAM, the experience however, may not be the best when there's little available
memory.

Preloading is a "set and forget" operation. Configure it once, setting the
desired maximum number of preloaded qubes you would like to have preloaded,
and the system takes care of the rest, including filling in the gaps of used
preloaded qubes.

The only action required to preload a disposable is to set the feature
`preload-dispvm-max` on the disposable template you most use for generating
unnamed disposables. Consider a user that does a lot of management operations
with `qvm-console-dispvm`. The default qube for those operations is
`default-mgmt-dvm`:

[![The qube's settings window of default-mgmt-dvm is open, disposable template checkbox is enabled and preloaded disposables spinbox has the value set to 2.](/attachment/posts/preload-local.png)](/attachment/posts/preload-local.png)

Or use the equivalent command-line operation:

```shell
qvm-features default-mgmt-dvm preload-dispvm-max 2
```

If you use the global `default_dispvm` a lot, you can target the global
preload setting by setting the feature on `dom0`:

[![The Global settings window is open, the default disposable template is default-dvm, the preload disposable qubes checkbox is enabled and the spinbox value is set to 2. The amount of free system memory for preloaded disposables remains the default.](/attachment/posts/preload-global.png)](/attachment/posts/preload-global.png)

Or use the equivalent command-line operation:

```shell
qvm-features dom0 preload-dispvm-max 2
```

While the disposable template remains the global `default_dispvm`, it will
respect the global feature and ignore the per-qube setting. This is useful as
it allows changing the global property to another disposable template while
retaining the same number of preloaded disposables. In order to allow the
global `default_dispvm` to respect the per-qube setting, delete the feature by
unchecking the box.

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
At a later moment, it will retry and preload gap will be filled if there is
enough memory. On a fresh install, this value is already set according to a
percentage of your system's total memory. A value equal or over 1000 MiB is
recommended.

When you consume/request a preloaded disposable, the next generation of
preload is delayed, this guarantee that a few quick requests have very fast
response time, this delay is ideal for request a browser, or sanitize a file,
but just up to the quantity of `preload-dispvm-max`. On the other hand, if you
intend to consume all preloaded disposables, such as when using Qubes Builder
V2, it is recommended to disable such delay:

```shell
qvm-features qubes-builder-dvm preload-dispvm-delay 0
```

## Performance improvements

The following graphs were sources from a specific [openQA
test](https://openqa.qubes-os.org/tests/199342#downloads), and were made to a
system with the following specs:

- **Date**: 2026-10-13T06:42:14
- **Qubes**: R4.4.1
- **Xen**: 4.19.6
- **Linux**: 6.18.55-1.25.fc41
- **Template**: Fedora 44 Xfce (build time: 2026-09-24 14:32:36)
- **Memory**: 16GB RAM
- **CPU**: 13th Gen Intel(R) Core(TM) i3-1315U
- **Drive**: 5153071B0DB700012871 Star Drive PCIe SSD

It is recommended to check the latest
[results](https://openqa.qubes-os.org/group_overview/6), searching for the
test `system_tests_dispvm_perf`, clicking on `Logs & Assets` and checking out
the graphs.

### Stacked stages

The stacked graph is an average of each stage across multiple runs. Such
visualization is used to evaluate which stages are taking most of the time,
when comparing against other tests with and without preload, and with and
without GUI connections.

First thing we note is the startup time of a disposable (red), notably taking
most of the time of all stages.

We later notice that creation of a disposable (magenta) without a GUI is super
fast, while when having a GUI and preloading, it takes considerable more time.
If your disposable workflow doesn't require a GUI, it is recommended to
disable the `gui` qube feature to speed things up.

If we look at the X times faster (green), we notice that preloading is on
average, faster than not using it.

Execution time of preloaded versus without it is skewed on this graph, we will
check it later on the line graph.

Cleanup time is a lot higher when there are preloaded disposables, possibly
due to some storage lock and the overhead of preloading another disposable
while one is cleaned.

<!--
TODO: ben: update graphs with latest OpenQA info, with faster response time

https://openqa.qubes-os.org/tests/199342/file/system_tests-dispvm_perf-fedora-44-xfce_graph_02_stage_stack_1_until_total.png
-->

[![Stacked stages](/attachment/posts/preload-graph-stacked.png)](/attachment/posts/preload-graph-stacked.png)

### Response distribution

This graph shows the time it takes for a qube to execute code on a disposable
or even a normal that is already running.

First thing we note is there are a few cases that executing code on a
preloaded disposable takes longer than just requesting a disposable, notice
the blue spread, mostly due to more moving parts, more storage locks, more of
everything simultaneously.

With regards to the mean response time of preloaded disposables, it is much
lower than without them.

The last two columns of this graph shows a qube requesting another running
qube. It doesn't bring the same level of security of using disposables, but it
is interesting to see that the response time is constant, compared to using
disposables or preloaded disposables. Comparing them against preloaded
disposables, we note that frequently, the dots appear at the same height
(time/speed), so it seems that preloaded disposables feels near to using a
normal running qube for your tasks.

[![Response distribution](/attachment/posts/preload-graph-dist-resp.png)](/attachment/posts/preload-graph-dist-resp.png)

### Response time per iteration

The tests was conduced by making calls that require a GUI. Several tests
configuration were made and the legend has details of the averages, including
of the first requests and considering all of them.

The response time of requesting a running qube (not preloaded disposables), is
constant, and very low, `0.02` seconds.

When analyzing the execution time per iteration of preloaded disposables, the
first request seems to be stable, at around `0.11-0.14` seconds (`110-140
milliseconds`), and it stays closer to that until the first batch of preloaded
disposables is consumed. This value is 3-4 times more than requesting a non
pristine running qube, but still low enough for our needs, this information
was not available in the distribution graph, but now we can clearly calculate
the difference.

Some prominent spikes happened because there were no preloaded disposables
ready to fulfill the large amount of requests. One of the reasons the spikes
are so high is the default delay between a disposable being generated post
one's destruction, this delay is optimized for workflows that request just a
few preloaded disposables, it guarantees the first request to be completed
quickly. If your workflow is know to exhaust the preload list, consume more
than what is available, consider setting the `preload-dispvm-delay` feature to
`0`.

[![Response distribution](/attachment/posts/preload-graph-line-resp.png)](/attachment/posts/preload-graph-line-resp.png)

## Known issues

Preloaded disposables are paused when they finish preloading and haven't been
requested for use yet. Paused qubes still consume allocated memory, and this
stale state does not allow for [memory redistribution](/doc/qmemman/).
Therefore, on memory constrained environments, preloading will lower the
available memory that can be used by your system, up to a threshold, that can
be set on the Global Config.

