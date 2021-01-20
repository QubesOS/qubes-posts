---
layout: post
title: "GitLab-CI and reproducible builds progress"
categories: articles
author: Marek Marczykowski-Górecki
---

GitLab-CI migration
--------------------

As many of you are aware, we migrated from Travis-CI to GitLab-CI late last year. While the [direct reason][ci-thread] was a change in the Travis-CI terms of service, GitLab-CI gives us many additional benefits. Just to name a few:

 - A modern execution environment with native Docker support: We can use whatever base environment we like. We are no longer constrained to specific (not so fresh) Ubuntu versions.
 - A lot more flexible job definitions, including dependencies among them: We use this to split jobs into smaller pieces that can run in parallel and reduce duplication among them.
 - Out-of-the-box support for cache and artifacts: Another feature allowing for a great speed-up of our tests. A specific build environment can be stored with a pre-populated cache, for example avoiding the need to create a chroot environment each time.
 - Higher time limits and the ability to connect our own workers: This allows us to automatically test bigger components like the Linux kernel (which previously didn't fit into Travis-CI's hard time limit).

The actual migration was a massive undertaking, with the [GitLab-CI configuration] spread across 50 files with over 1000 lines in total. We have opened and merged over 90 pull requests in the process. This was mainly done by [Frédéric Pierret].

We still host the actual code on GitHub. We use GitLab only for CI. This mode of operation is supported natively by GitLab, but this support is quite limited. Most importantly, it [does not support] testing pull requests made from repository forks, which is the vast majority of our pull requests (if not all of them). For this reason, Frédéric ended up creating [our own integration], which takes care of uploading the code to GitLab, scheduling CI jobs, and reporting the results back to GitHub. In fact, we are not the only project that has taken this route. Several other similar projects, such as check_pr, coq bot, and labhub, have also done so.

In the process, our CI has gained some nice extra features:

- Control via special comments in pull requests:
  - `PipelineRetry` --- restarts a CI job
  - `PipelineRefresh` --- refreshes job status in GitHub (should normally not be needed)
  - `testdeploy` --- in a website-related pull request, takes the website built in GitLab-CI and publishes it under a temporary address (works only after the GitLab-CI build completes and only for selected users)
- More tests, specifically for reproducible builds of packages

We also have a nice [summary page] with all the recent jobs states, which is updated daily.


Reproducible builds
--------------------

Another major work item is progressing [reproducible builds] in Qubes OS. Our current short-term goal is to integrate what is realistically possible in Debian. Specifically, we aim to get our Debian templates to the state in which packages can be installed only when configured rebuilders confirm they really came from the source code we publish. For this task, we focus specifically on Debian Bullseye (testing version), which includes most of the reproducibility work done by the Debian community.

To start, we had to bring our packages to the state in which they could be built reproducibly. GitLab-CI massively helped here with identifying issues. One of the CI jobs we added is running the `reprotest` tool on our packages. This tool builds the package twice in two different build environments and compares the results. If there are any differences, the job automatically runs the `diffoscope` tool, which provides a detailed, in-depth report.
Now, all our packages (for Debian Bullseye) are reproducible, and we are actively monitoring it with our CI. This can be seen in the "reproducible" badges in the CI [summary page].

The next step on the journey is enabling an arbitrary user or organization to challenge the official packages --- to take the sources, together with the `buildinfo` file describing how it was built and verify that they result in the same binary package. Theoretically, there is a [`debrebuild.pl`] script in Debian's `devscripts` package that does exactly this. We started by adding missing parts to it, but it turned out there were several limitations making it unsuitable for our use case. The primary limitation is support for only one source repository. (We need two: original Debian and Qubes.) Since `debrebuild.pl`, as the name suggests, is written in Perl, it isn't very friendly for contributing to. For these reasons, we decided to write our own similar tool in Python. While this was a bit of work, [the end result][debrebuild] is very nice: about 900 lines of code, with proper structure using classes. We are in the process of upstreaming this code back to Debian, potentially to replace the original `debrebuild.pl` tool.

To ease the rebuilder setup, we are also in the process of creating a simple [rebuilder orchestrator] that monitors specified repositories and does the package build verification automatically when necessary. The goal is to give anyone who wants to verify our (or others) packages an easy way to get started. The decision of who to trust is separate, but in order to have a choice, rebuilders must exist in the first place. This also includes scenarios in which users or organizations want to verify packages for themselves. 

When the package rebuild is done, and it matches the official package, the next step is to announce this result so other users can take it into account when deciding whether to install the package or not. For this purpose, we are using the [in-toto] framework with its APT integration, [apt-transport-in-toto]. This step requires publishing signed hashes of rebuilt packages in a specific format (so-called "link" files). We have integrated this into our `debrebuild` tool (and the original `debrebuild.pl` too). The result can be seen at [Frédéric's rebuilder site].

The next step is to configure `apt-transport-in-toto` to actually use these proofs, but this step depends on the user to decide who to trust. We will provide some sensible defaults, as well as instructions for how to configure one's own trust policies.


Next steps, RPM support
------------------------

As explained above, some parts still need finishing, as well as cleanups and proper documentation. But we are very close to the point where every Debian package we build can be independently verified. The very same tooling we've made can be used to verify native Debian packages too, which should also be helpful for non-Qubes Debian users.

While we have started with Debian, our goal is to have all of our packages reproducibly built and independently verified. We have started some work on the RPM packages already. The very first thing is to add reproducibility monitoring and reporting to GitLab-CI. For this reason, Frédéric has already added [RPM support to the `reprotest` tool]. When the new version of `reprotest` is released, we will add RPM package testing too.


Acknowledgements
-----------------

This work is possible thanks to [generous support][moss-award] from [Mozilla Open Source Support (MOSS)].

I'd like also to thank GitLab for granting us a free GitLab Gold license, which enabled much higher service quotas.


[ci-thread]: https://www.mail-archive.com/qubes-devel@googlegroups.com/msg04698.html
[GitLab-CI configuration]: https://github.com/qubesos/qubes-continuous-integration
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
[RPM support to the `reprotest` tool]: https://salsa.debian.org/reproducible-builds/reprotest/-/merge_requests/10
[moss-award]: /news/2020/05/22/moss-mission-partners-award/
[Mozilla Open Source Support (MOSS)]: https://www.mozilla.org/en-US/moss/
