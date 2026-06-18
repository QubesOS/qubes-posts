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
[usage section](#How to use preloaded disposables).

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

Hyperscalers also have this concept, although each use a different name and
method:

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

## Performance improvements

The following tests were conducted on a system with:

- **Date**: 2025-11
- **Qubes**: R4.3-rc2
- **Xen**: 4.19.3
- **Linux**: 6.15.11-1.fc41
- **Template**: Fedora 42 Xfce (build time: 2025-08-19 23:42:23)
- **Memory**: 32GB RAM
- **CPU**: Intel(R) Core(TM) Ultra 7 155H
- **Drive**: PX700 NVMe SSD

Note that these results were made months before this post and improvements
were made in order to speed up the response time even more, although some of
them did not make in time to this stable release.

<!--
TODO: ben: update graphs with latest OpenQA info, with faster response time
-->

### Stacked execution

The average execution time is very low, even considering a great amount of
consecutive iterations. The GUI execution is unfortunately higher because the
GUI daemon only starts the connection with the qube's GUI agent when the
preload disposable is marked as used.

[![Stacked execution](/attachment/posts/preload-graph_01_stage_stack_0.png)](/attachment/posts/preload-graph_01_stack_stack_0.png)

### Stacked total

Notice that execution time is not all that counts, disposables are destroyed
after use, this time has to be considered to measure the call performance.
Notice that preloaded disposables cleanup are higher than their normal
variants, this is due to a new preload being created while the current one was
marked as used.

[![Stacked total](/attachment/posts/preload-graph_01_stage_stack_1.png)](/attachment/posts/preload-graph_01_stack_stack_1.png)

### Execution per iteration

When analyzing the execution time per iteration, the first request seems to be
stable, at around `0.3` seconds (`300 milliseconds`). Some prominent spikes on
iterations 9-11 happened because there were no preloaded disposables ready to
fulfill the large amount of requests. Nevertheless, it was still faster than
normal disposables. If your workflow doesn't require a large amount of
preloaded disposables in a short period, 1-3 preloaded disposables might be
enough (the graph doesn't show 1-2 preloaded because it would have too many
overlapping lines).

[![Line execution](/attachment/posts/preload-graph_03_line_0_exec.png)](/attachment/posts/preload-graph_03_line_0_exec.png)

### Total per iteration

At last, there are no occurrences of preloaded disposables underperforming
normal disposables.

[![Line total](/attachment/posts/preload-graph_03_line_2_total.png)](/attachment/posts/preload-graph_03_line_2_total.png)

## Known issues, some soon to be fixed

Preloaded disposables are paused when they finish preloading and haven't been
requested for use yet. Paused qubes still consume allocated memory, and this
stale state does not allow for [memory redistribution](/doc/qmemman/).
Therefore, on memory constrained environments, preloading will lower the
available memory that can be used by your system, up to the threshold, that
can be seen on the Global Config.

The GUI connection to preloaded disposables is delayed until they are used for
various reasons such as:

- Preloads can be started and paused before the GUI session starts;
- Autostarted applications won't display as they would become unresponsive
  when the qube suddenly pauses.

Ideally, the GUI connection would not be delayed by seconds. Your wishes may
come true in the next release, [work is being done to make GUI requests as fast
as headless ones](https://github.com/qubesos/qubes-issues/issues/9940).

## Industry comparison of pristine environments and reducing cold start latency

Cold start latency is the time it takes for the server to respond
the request made by the client, when it needs to start something, in this
case, a pristine environment.

Fast response times worries every user and service provider, the provider
being your computer or hyperscalers. Various projects uses different methods
to run code on an isolated environment, sometimes to run untrusted code, other
times to access private data securely. Some implementations allow reducing
cold start.

The information disposed below was fetched on a best effort basis, as most of
the times is is spread across user documentation, developer documentation,
blog posts and third party benchmarks. Besides that, even when information is
provided by some vendors, it is not public knowledge of specific numbers,
sometimes it is referred to as "sub-second" or "tens of thousands".

This post won't be updated when the information is updated elsewhere. Consult
upstream for the latest documentation.

### Pristine environments that works on a laptop

**Preloaded disposables**:

- **Technology**: Paused Xen domains queue.
- **Usage**: Anything, but as it is mostly targeted at desktop users, many
  concurrent requests might be a bit slower due to overhead of provisioning
  concurrency (preloading) and then removing the VM disks on cleanup.
- **Speed**:
  - Response time is the time it takes for the Qrexec agent on the disposable
    to reply. Total time is the time it takes to finish the operation, which
    includes cleaning up the domain. The GUI time is currently slow, but
    changes are in the way to make it as fast as without GUI.
  - It is recommended to check the latest
    [results](https://openqa.qubes-os.org/group_overview/6), searching for the
    test `system_tests_dispvm_perf`, clicking on `Logs & Assets` and checking
    out the graphs.
  - **1st request without GUI**: 100-150ms (response), 900-950ms total.
  - **1st request with GUI**: 1100-1300ms (response), 2000-2300ms total.
  - The following benchmarks don't try to max out the results by provisioning
    as much as needed, it chose 4 preloaded disposables as something that can
    be handled by a laptop with 16-32GB of RAM alongside other running qubes.
  - **12 requests without GUI (4 preloaded)**: 600-1500ms (response),
    3000-3200ms (total).
  - **12 requests with GUI (4 preloaded)**: 1900-2100ms (response),
    3400-3600ms (total).
- **Limitations**:
  - Overhead cannot be offloaded from the user machine, volume cleanup on
    domain shutdown must happen at some point and it slow: 0.9-4s
  - More qubes running at the same time means more memory being unavailable
    for redistribution.

**Xen Project**:

- **Technology**: [Xen domain forking](https://doc.qubes-os.org/en/latest/developer/services/disposablevm-implementation.html#xen-domain-fork).
- **Usage**: Fuzzing.
- **Speed**: [0.745ms per fork](https://www.youtube.com/watch?v=3MYo8ctD_aU).
  No information about request speed or total time including deleting a
  domain. Depends on the filesystem used and event handlers.
- **Limitations**:
  - Not designed for long running sessions.
  - Memory is mapped on request with Copy-on-Write.

[Restoration from savefile](https://doc.qubes-os.org/en/latest/developer/services/disposablevm-implementation.html#restoration-from-savefile):

- **Technology**: Most hypervisors, image dump (suspend to disk) and
  restoration of this image.
- **Usage**: Same as preloaded disposables, was active in Qubes OS R3.2 but
  dropped due to limitations explained below.
- **Speed**: Less than 1000ms.
- **Limitations**:
  - Memory issues arise after some undetermined time.
  - Incompatible with multiple `vcpus`.
  - Savefile creation takes a long time. The disposable qube savefile contains
    references to template rootfs and CoW files, if there is a modification on
    the template or disposable template, it took longer than 2.5 minutes to
    generate the next disposable.

### Pristine environments for hyperscalers

**Amazon Web Services (AWS)**:

- **Technology**: KVM - [Firefracker
- **Usage**: [File processing, Long-running workflows, databases, HTTP streams](https://aws.amazon.com/blogs/opensource/firecracker-open-source-secure-fast-microvm-serverless/):
  [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html): MicroVM
  KVM
- [Limitations](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html#managing-provisioned-concurency):
  - **Max duration**: 900 seconds.
  - **Max memory**: 128 MB - 10.240 MB.
  - **Max concurrency**: 1000 - Tens of thousands.
  - **Max storage**: 75 GB - Terabytes.
  - **Native code execution**: No, has managed runtimes for Python, Node.js,
    .NET and Ruby. Has OS-only runtimes but requires using Amazon Linux.
  - **CPU power**: It is allocated proportionally to memory, 1769 MB is
    equivalent to 1 vCPU. Unsure if one can be set independent of the other.
- **Speed**:
  - Depends on implementation.
  - OS-only runtimes benchmark could not be identified.
- **Implementations**:
  - [SnapStart](https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html)
    - **Description**: Restores cached snapshot of memory and disk state.
    - **Speed**: less than 1000ms.
    - **Limitations**:
      - Not all language runtimes are supported.
      - OS-only and container runtimes are also unsupported.
  - [Provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html)
    - **Speed**: 10-99ms.
    - **Autoscaling**: [Yes](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html#managing-provisioned-concurency)

**Cloudflare**:

- [Technology](https://developers.cloudflare.com/workers/reference/how-workers-works/)
  [Workers](https://blog.cloudflare.com/eliminating-cold-starts-with-cloudflare-workers/):
  Google Chrome V8 JavaScript engine.
- [Usage](https://tutorial.cloudflareworkers.com/): Modify a site's HTTP
  requests and responses, make parallel requests, and reply.
- [Speed](https://blog.cloudflare.com/unpacking-cloudflare-workers-cpu-performance-benchmarks/):
  Next.js (829ms), React SSR (126ms), SveltKit (79ms), Vanilla JS (132ms)
- [Security model](https://developers.cloudflare.com/workers/reference/security-model/)
- [Limitations](https://developers.cloudflare.com/workers/platform/limits):
  - **Max memory**: 128 MB.
  - **Max duration**: infinite.
  - **Max CPU time**: 300 seconds.
  - **Native code execution**: No, languages have to be transpiled to
    JavaScript or WebAssembly to be passed to V8 for code execution.

**Google Cloud Platform (GCP)**

- **Technology**: [Cloud Run functions](https://docs.cloud.google.com/functions/docs) -
- **Usage**: Small code snippets that respond to events while the technology
  handles the operational infrastructure
  Containers.
- **Speed**: Couldn't identify. There is [PerfKitBenchmarker](https://googlecloudplatform.github.io/PerfKitBenchmarker/),
  but links from that page have broken redirection.
- [Limitations](https://docs.cloud.google.com/functions/quotas):
  - Detailed information in the link above.
  - **Max duration**: 3600 seconds for HTTP functions.
  - **Max memory**: 32 GiB.
  - **Native code execution**: Images provided by the vendor, only
    [google24/osonly24](https://docs.cloud.google.com/run/docs/runtime-support#os-only).

Notice that the hyperscalers use case are very different than Qubes OS ones.
While hyperscalers focus on HTTP requests and provide a minimal environment
for code execution, most of the times without operating system access. Qubes
OS users wants a full-fledged applications that runs on any operating system
or their liking, being it Linux, BSD or Windows, using browsers, file
managers, terminal emulators or any other software they choose. We don't
discard the possibility of reducing cold start latency of some Qrexec services
by making dedicated smaller images instead of requiring a full bloated Linux
distribution for file sanitization for example.

One thing that would be fruitful for preloaded disposables is that
hyperscalers uses is autoscaling, adapting the number of preloaded disposables
based on the number of recent requests, would require an algorithm to analyze
patterns. Something interesting for when Qubes OS start being widely deployed
for servers.
