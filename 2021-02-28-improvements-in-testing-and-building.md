---
layout: post
title: "Improvements in testing and building: GitLab CI and reproducible builds"
categories: articles
author: Marek Marczykowski-Górecki
---

Over the last couple of months, we have made some significant changes to two important parts of the Qubes development process: testing and building.


What are continuous integration (CI) and reproducible builds?
--------------------------------------------------------------

Automated testing is a major part of the software development process. It spares developers many, many hours of manual testing that would still miss some bugs and other problems. In Qubes development, we're using an approach called "continuous integration" (CI), in which local changes made by the developers are frequently merged and tested remotely, using dedicated automated testing solutions. This is very important both for maintaining consistent code quality (all changes are tested) and for making development easier for the developers. Testing Qubes is not easy. Since Qubes is an entire operating system, doing the testing on the same system in which you're developing is a bit like building a rocket landing system en route to Mars --- not impossible, but very stressful.

The second area of improvement is the build process. The term "[reproducible builds]" refers to a process in which the same source code always compiles into exactly the same binary (for example, a package used to install a program via a package manager like `dnf` or `apt`). Why is this difficult to achieve? After all, computers are not random. Shouldn't builds be reproducible by default, without requiring special effort to make them deterministic? Unfortunately, it's not that simple. There are thousands of variables influencing the way binaries are built, ranging from the time of day to the availability of remote servers and locale settings.

Ensuring that binaries are built the same way every time is surprisingly difficult. However, the effort is worth the security benefits. To understand these benefits, imagine that an attacker wishes to feed unsuspecting users a compromised package. The attacker knows that the source code is public, so any malicious code he inserts into it would be highly exposed and at risk of detection. On the other hand, he reasons, compromising the build infrastructure would allow him to surreptitiously insert malicious changes that would make it into the resultant package. Since the source code remains untouched, his malicious changes are less likely to be detected. This is where the value of reproducible builds comes in. If the build process is reproducible, then we will immediately notice that building a package from the untouched source code results in a package that is *different* from the compromised one. This would be a major red flag that would prompt an immediate security investigation.


GitLab-CI migration
--------------------

As many of you are aware, we migrated from Travis-CI to GitLab-CI late last year. While the [direct reason][ci-thread] was a change in the Travis-CI terms of service, GitLab-CI gives us many additional benefits. Just to name a few:

 - A modern execution environment with native Docker support: We can use whatever base environment we like. We are no longer constrained to specific (not so fresh) Ubuntu versions.
 - Much more flexible job definitions, including dependencies among them: We use this to split jobs into smaller pieces that can run in parallel and reduce duplication among them.
 - Out-of-the-box support for caching and artifacts: Another feature allowing for a great speed-up of our tests. A specific build environment can be stored with a pre-populated cache, for example avoiding the need to create a chroot environment each time.
 - Higher time limits and the ability to connect our own workers: This allows us to automatically test bigger components like the Linux kernel (which previously didn't fit into Travis-CI's hard time limit).

The actual migration was a massive undertaking, with the [GitLab-CI configuration] spread across 50 files with over 1,000 lines in total. We have opened and merged over 90 pull requests in the process. This was mainly done by [Frédéric Pierret].

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

Our current short-term goal for reproducible builds in Qubes OS is to integrate what is realistically possible in Debian. Specifically, we aim to get our Debian templates to the state in which packages can be installed only when configured rebuilders confirm they really came from the source code we publish. (A "rebuilder" is a program that takes a binary and its purported source code as inputs, along with any applicable metadata, and attempts to build an identical binary from the source code. The goal is to check whether the binary was really compiled from its claimed source code.) For this task, we focus specifically on Debian Bullseye (testing version), which includes most of the reproducibility work done by the Debian community.

To start, we had to bring our packages to the state in which they could be built reproducibly. GitLab-CI massively helped here with identifying issues. One of the CI jobs we added is running the `reprotest` tool on our packages. This tool builds the package twice in two different build environments and compares the results. If there are any differences, the job automatically runs the `diffoscope` tool, which provides a detailed, in-depth report.
Now, all our packages for Debian Bullseye are reproducible, and we are actively monitoring it with our CI. This is designated by the "reproducible" badge on the CI [summary page].

The next step on the journey is to enable an arbitrary user or organization to challenge the official packages --- to take the sources, together with the `buildinfo` file describing how it was built and to verify that they result in the same binary package. Theoretically, there is a [`debrebuild.pl`] script in Debian's `devscripts` package that does exactly this. We started by adding missing parts to it, but it turned out there were several limitations making it unsuitable for our use case. The primary limitation is support for only one source repository. (We need two: original Debian and Qubes.) Since `debrebuild.pl`, as the name suggests, is written in Perl, it isn't very friendly for contributing to. For these reasons, we decided to write our own similar tool in Python. While this was a bit of work, [the end result][debrebuild] is very nice: about 900 lines of code, with proper structure using classes. We are in the process of upstreaming this code back to Debian, potentially to replace the original `debrebuild.pl` tool.

To ease the rebuilder setup, we are also in the process of creating a simple [rebuilder orchestrator] that monitors specified repositories and does the package build verification automatically when necessary. The goal is to give anyone who wants to verify our (or others') packages an easy way to get started. The decision of who to trust is separate, but in order to have a choice, rebuilders must exist in the first place. This also includes scenarios in which users or organizations want to verify packages for themselves. 

When the package rebuild is done, and it matches the official package, the next step is to announce this result so other users can take it into account when deciding whether to install the package or not. For this purpose, we are using the [in-toto] framework with its APT integration, [apt-transport-in-toto]. This step requires publishing signed hashes of rebuilt packages in a specific format (so-called "link" files). We have integrated this into our `debrebuild` tool (and the original `debrebuild.pl` too). The result can be seen at [Frédéric's rebuilder site].

The next step is to configure `apt-transport-in-toto` to actually use these proofs, but this step requires the user to decide who to trust. The configuration process is currently described in the README of [rebuilder orchestrator]. It includes steps to use both Frédéric's rebuilder as well as to create a custom configuration. When enabled, every installed package is checked to ensure that a rebuilder can confirm that the package was really compiled from its purported source code. For example, installing the `tzdata` package looks like this:

```
root@debian-11:~# apt-get install tzdata
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be upgraded:
  tzdata
1 upgraded, 0 newly installed, 0 to remove and 842 not upgraded.
Need to get 284 kB of archives.
After this operation, 4,096 B of additional disk space will be used.
Get:1 intoto://deb.debian.org/debian bullseye/main amd64 tzdata all 2021a-1 [284 kB]
(...)
In-toto verification for '/var/cache/apt/archives/partial/tzdata_2021a-1_all.deb' passed! :)
Fetched 284 kB in 15s (19.3 kB/s)
Reading changelogs... Done
(...)
```


Reproducible RPMs
------------------

While we started with Debian, our goal is to have all of our packages reproducibly built and independently verified. Reproducible builds support in the RPM ecosystem is mainly driven by OpenSUSE, but Fedora and other RPM-based distributions benefit from this too. In some cases, this means that Fedora just needs some configuration to be adjusted in order to achieve a reproducible build, while in other cases this requires developing missing tools.

First of all, there is a set of tools that is very helpful while working on reproducible builds, specifically:

 - `diffoscope` --- compares files in depth; an indispensable tool when working with packages and other compound file formats
 - `reprotest` --- builds a package twice and compares the results
 - `disorderfs` --- intentionally disturbs the way in which the filesystem is visible to applications, especially the order in which files are listed

While `diffoscope` was already available in Fedora, the other two weren't. Frédéric started by adding RPM support to `reprotest` and then packaging both `disorderfs` and `reprotest` for Fedora. Thanks to these efforts, all three tools are now available in the default Fedora repositories.

Next, similarly to Debian, we developed a tool that takes an official package and tries to reproduce it. We call it [`rpmreproduce`]. While working on it, we noticed that there is no single official format for an RPM package build environment description, something that has existed in Debian for a long time as the `buildinfo` format. After discussing it a bit with the community, we submitted a [`buildinfo`][rpm buildinfo] proposal and implementation to the upstream RPM project. At this stage, the only thing left to do is to add it to the rebuilder orchestrator mentioned earlier.

Having a package rebuilder running for RPM packages is one thing. Actually using it to verify packages during installation is another matter. In Debian, we used an existing APT plugin (`apt-transport-in-toto`), but for Fedora no such thing existed. To fill this gap, we have developed [`dnf-plugin-in-toto`] which works on a very similar basis. The DNF plugin architecture is a bit different from that of APT. It allows DNF to request a rebuild confirmation (based on the package hash from the repository metadata) before actually downloading package. There is still some work to do on this plugin, but the basic functionality is there already. Here's an example:

```
# dnf install --enablerepo=qubes*testing qubes-u2f
(...)
Prepare in-toto verification for 'qubes-u2f-1.2.8-1.fc33.noarch'
Create verification directory '/tmp/tmpfei9swbm'
Request in-toto metadata from 1 rebuilder(s) (DNF global_info)
Request in-toto metadata from https://mirror.notset.fr/qubes/rebuild/yum/r4.1/vm/sources/qubes-u2f/1.2.8-1.fc33/metadata
Successfully downloaded in-toto metadata 'rebuild.8deb0bef.link' from rebuilder 'https://mirror.notset.fr/qubes/rebuild/yum/r4.1/vm/'
Copy final product to verification directory
Load in-toto layout '/home/user/dnf-transport-in-toto/data/root.layout' (DNF global_info)
Load in-toto layout key(s) '['9fa64b92f95e706bf28e2ca6484010b5cdc576e2']' (DNF global_info)
Use gpg keyring '/home/user/dnf-transport-in-toto/data/gnupg' (DNF global_info)
Run in-toto verification
In-toto verification for 'qubes-u2f-1.2.8-1.fc33.noarch' passed! :)
Dependencies resolved.
=======================================================================================
 Package                Arch    Version            Repository                      Size
=======================================================================================
Installing:
 qubes-u2f              noarch  1.2.8-1.fc33       qubes-vm-r4.1-current-testing  264 k
Installing dependencies:
 hidapi                 x86_64  0.9.0-4.fc33       fedora                          45 k
 python3-cryptography   x86_64  3.2.1-2.fc33       updates                        546 k
 python3-hidapi         x86_64  0.9.0.post2-2.fc33 fedora                          50 k
 python3-u2flib-host    noarch  3.0.3-9.fc33       qubes-vm-r4.1-current-testing   49 k

Transaction Summary
=======================================================================================
Install  5 Packages

Total download size: 954 k
Installed size: 3.6 M
```

Next steps
-----------

As explained above, some parts still need finishing, as well as cleanups and proper documentation. But we are very close to the point where every Debian package we build can be independently verified. The very same tooling we've made can be used to verify native Debian packages, too, which should also be helpful for non-Qubes Debian users. Similar progress has already been made for Fedora, although some more work is needed on the Fedora side to allow reproducing native (not only Qubes-related) packages.

In the broader future, our ultimate goal is to make all parts of Qubes OS reproducible. This includes template and the installation image too. Reproducible packages are the first step to this goal, which incidentally is also most valuable to our users and broader community.


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
[rebuilder orchestrator]: https://github.com/fepitre/package-rebuilder/
[in-toto]: https://in-toto.io/
[apt-transport-in-toto]: https://github.com/in-toto/apt-transport-in-toto/
[Frédéric's rebuilder site]: https://mirror.notset.fr/qubes/rebuild/deb/r4.1/vm/sources/
[RPM support to the `reprotest` tool]: https://salsa.debian.org/reproducible-builds/reprotest/-/merge_requests/10
[`rpmreproduce`]: https://github.com/fepitre/rpmreproduce
[rpm buildinfo]: https://github.com/rpm-software-management/rpm/pull/1532
[`dnf-plugin-in-toto`]: https://github.com/fepitre/dnf-plugin-in-toto
[moss-award]: /news/2020/05/22/moss-mission-partners-award/
[Mozilla Open Source Support (MOSS)]: https://www.mozilla.org/en-US/moss/
