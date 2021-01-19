---
layout: post
title: "Gitlab-CI and reproducible builds progress"
categories: articles
author: Marek Marczykowski-Górecki
---

Gitlab-CI migration
--------------------

Many of you are aware we have migrated from Travis-CI to Gitlab-CI late last year. While the [direct reason][ci-thread] was a change in Travis-CI terms of service, Gitlab-CI gives us a lot more than we have on Travis-CI. Just to name a few:

 - modern execution environment, with native docker support - we can use whatever base environment we like, we are no longer constrained to specific (not so fresh) Ubuntu versions
 - a lot more flexible jobs definitions, including dependencies between them - we use this to split jobs into smaller pieces that can run in parallel, and reduce duplication between them
 - out of the box support for cache and artifacts - another feature allowing a great speed up of the tests - specific build environment can be stored with pre-populated cache, for example avoiding the need to create chroot environment each time
 - higher time limits and ability to connect our own workers - this allows us to automatically test also bigger components like Linux kernel (where the build didn't fit in Travis-CI hard time limit)

The actual migration was a massive work item, the [Gitlab-CI configuration] is spread across 50 files with over 1000 lines in total. We have opened and merged over 90 pull requests in the process. This was mainly done by [Frédéric Pierret].

We still host the actual code at Github and we use Gitlab for CI only. This mode of operation is supported natively by Gitlab, but this support is quite limited. Most importantly, it [does not support] testing pull requests made from repository forks - which is the great majority of our pull requests (if not all of them). For this reason, Frédéric ended up creating [our own integration], which takes care about uploading the code to Gitlab, scheduling CI jobs and reporting results back to Github. In fact, we are far not the only ones that have gone this way, there are several other similar projects (check_pr, coq bot, labhub, ...).

In the process, our CI got nice extra features:

- controlling via special comments in pull requests:
  - `PipelineRetry` - will restart a CI job
  - `PipelineRefresh` - refresh job status in github (should not be needed normally)
  - `testdeploy` - in a website-related pull request, will take the website built in Gitlab-CI and publish under a temporary address (this comment works only after Gitlab-CI build completes, and only for selected users)
- more tests, specifically test reproducible builds of packages

We have also nice [summary page] with all the recent jobs states - it's updated daily.


Reproducible builds
--------------------

Another major work item is progressing [reproducible builds] in Qubes OS. Our current short-term goal is to integrate what is realistically possible in Debian. Specifically, to get our Debian templates to the state when packages can be installed only when configured rebuilders confirm they really came from the source code we publish. For this task we focus specifically on Debian bullseye (testing version), which includes most of the reproducibility work done by the Debian community.

For start, we needed to bring our packages to the state when they can be build reproducibly. Gitlab-CI massively helped here with identifying issues - one of the CI jobs we added is running `reprotest` tool on our packages. This tool build the package twice, in two different build environments and compare results. In case of differences, automatically runs `diffoscope` tool, which provides detailed, in-depth, report where is the issue.
Now, all our packages (for Debian bullseye) are reproducible and we are actively monitoring it with our CI. This can be seen in "reproducible" badges in the CI [summary page].

The next step on the journey is enabling arbitrary user/organization to challenge the official packages - take the sources, together with `buildinfo` file describing how it was built and verify if they result in the same binary package. Theoretically, there is [`debrebuild.pl`] script in Debian's `devscripts` package that do exactly this. We started by adding missing parts to it, but it turned out there are several limitations making it unsuitable for our use case - the primary one being support for one source repository only (we need two: original Debian's one and the Qubes's one). Since the `debrebuild.pl`, as the name suggests, is written in Perl, it isn't very friendly for contributing to. Because of the above reasons, we have decided to write our own similar too - in Python. While this was a bit of work, [the end result][debrebuild] is very nice - about 900 lines of code, with proper structure using classes. We are in the process of upstreaming this code back to Debian, helpfully replacing original `debrebuild.pl` tool.

To ease the rebuilder setup, we are also in process of creating simple [rebuilder orchestrator] that monitors specified repositories and do the package build verification when necessary automatically. The goal is to enable anyone who wants to easily start verifying our (or others) packages. The decision who to trust is separate, but to have a choice - rebuilders need to exists in the first place. This includes also scenarios when user/organization wants to verify packages for themselves. 

When the package rebuild is done and it matches the official one, the next step is to announce this result so other users can take it into account when deciding whether to install the package or not. For this purpose we are using [in-toto] framework, together with its APT integration - [apt-transport-in-toto]. This step requires publishing signed hashes of rebuilt packages in a specific format (so called "link" files). We have integrated this into our `debrebuild` tool (and the original `debrebuild.pl` too). The result can be seen at [Frédéric's rebuilder site].

The next step is to configure `apt-transport-in-toto` to actually use those proofs - but this step depends on the user choice who to trust. We will provide some sensible defaults and also instructions how to configure own trust policies.


Next steps, RPM support
------------------------

As said above, some of the parts need finishing, cleanups and proper documentation. But we are very close to the point where every deb package we built can be independently verified. The very same tooling we made can be used to verify native Debian packages too, which should be usable also for non-Qubes Debian users.

While we have started with Debian, our goal is to have all the packages build reproducibly and independently verified. We have started some work on the RPM packages already. The very first thing is to add reproducibility monitoring and reporting to Gitlab-CI and for this reason Frédéric already added [RPM support to the reprotest tool]. When the new version of `reprotest` gets released, we will add RPM packages testing too.


Acknowledges
-------------

This work is possible thanks to [generous support][moss-award] from [Mozilla Open Source Support (MOSS)].

I'd like also to thank Gitlab for granting us free Gitlab Gold license, which enabled much higher service quotas.

[ci-thread]: https://www.mail-archive.com/qubes-devel@googlegroups.com/msg04698.html
[Gitlab-CI configuration]: https://github.com/qubesos/qubes-continuous-integration
[Frédéric Pierret]: /team/#frédéric-pierret
[does not support]: https://gitlab.com/gitlab-org/gitlab/-/issues/5667
[our own integration]: https://github.com/fepitre/qubes-g2g-continuous-integration
[summary page]: https://qubesos.gitlab.io/qubes-g2g-report/
[reproducible builds]: https://reproducible-builds.org/
[`debrebuild.pl`]: https://salsa.debian.org/debian/devscripts/-/blob/master/scripts/debrebuild.pl
[debrebuild]: https://github.com/fepitre/debrebuild
[rebuilder orchestrator]: https://github.com/fepitre/qubes-rebuilder/
[in-toto]: https://in-toto.io/
[apt-transport-in-toto]: https://github.com/in-toto/apt-transport-in-toto/
[Frédéric's rebuilder site]: https://mirror.notset.fr/qubes/rebuild/deb/r4.1/sources/
[RPM support to the reprotest tool]: https://salsa.debian.org/reproducible-builds/reprotest/-/merge_requests/10
[moss-award]: /news/2020/05/22/moss-mission-partners-award/
[Mozilla Open Source Support (MOSS)]: https://www.mozilla.org/en-US/moss/
