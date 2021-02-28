---
layout: post
title: "Improvements in testing and building: GitLab CI and reproducible builds"
categories: articles
author: Marek Marczykowski-Górecki
---

Over the last couple of months, we had made some significant changed to two important parts of Qubes development process: testing and building. If terms like CI or reproducible builds are clear and obvious to you, you can jump to detailed technical explanations below - this introduction will give a little more context to those of our readers who may be encountering those terms for the first time.

Automated testing is a huge part of the software development process. It may not be very visible on the outside, but in practice, it is crucial and spares developers many, many hours of manual tests that would still often miss bugs and problems. In Qubes development, we're using an approach called Continuous Integration (CI) - local changes made by the developers are uploaded and tested remotely, using dedicated CI solutions. This is very important for both maintaining consistent code quality (all changes will be tested) and for making development easier for the developers (testing Qubes is not easy - it's a whole operating system, and doing the testing on the same system you're developing in is a bit like building a rocket landing system en route to Mars... perhaps not impossible, but very stressful).

The second part we made improvements in is the build process. The idea of 'reproducible builds' may sound quite silly at first - it just means that the same source code results in exactly the same executable program, and what exactly would be difficult here? After all, computers are not random - right? Unfortunately, nothing is simple. There are thousands of variables influencing the way programs are built, ranging from time of day to availability of remote servers and locale settings. Ensuring that programs are built the same way every time is surprising difficult, but it's a great way of establishing a chain of trust in which you, the end user, can be certain the program you're running is the same program the developer compiled, and that it doesn't have any malicious additional content added at some point in distribution.


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

The next step is to configure `apt-transport-in-toto` to actually use these proofs, but this step depends on the user to decide who to trust. The configuration process is currently described in the README of [rebuilder orchestrator] - it includes steps to use both Frédéric's rebuilder, as well as to create custom configuration. When enabled, every installed package is checked if a rebuilder confirms it really comes from the source code it claims. For example with `tzdata` package it looks like this:

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

Reproducible RPM
-----------------

While we have started with Debian, our goal is to have all of our packages reproducibly built and independently verified. Reproducible builds support in RPM ecosystem is mainly driven by OpenSUSE, but Fedora and other RPM-based distributions benefit from this too. In some cases it means Fedora needs just some configuration to be adjusted to achieve a reproducible build, in other cases it requires developing missing tools.

First of all, there is a set of tools very helpful while working on reproducible builds, specifically:

 - `diffoscope` - compares files in depth - indispensable tool when working with packages and other compounds file formats
 - `reprotest` - builds a package twice and compares results
 - `disorderfs` - intentionally disturbs how filesystem is visible to applications, especially order in which files are listed

While diffoscope was already available in Fedora, the other two weren't. Frédéric started the work by adding rpm support to reprotest and then packaging both disorderfs and reprotest for Fedora. Thanks to this, all three tools are now available in default Fedora repositories.

Next, similarly to Debian, we have developed a tool to take an official package and try to reproduce it - we called it [rpmreproduce]. While working on it, we noticed there is no one official format of package build environment description - something that exists in Debian for a long time as "buildinfo" format. After discussing it a bit with the community, we submitted a [buildinfo][rpm buildinfo] proposal and implementation to the upstream rpm project. At this stage, the only thing left to do is to add it to the rebuilder orchestrator mentioned earlier.

Having package rebuilder running for RPM packages is one thing. Another is actually using it to verify packages during installation. In Debian we used already existing APT plugin (`apt-transport-in-toto`), but for Fedora no such thing existed. To fill this space, we have developed [dnf-plugin-in-toto] which works on a very similar basis. DNF plugin architecture is a bit different than APT, which allows DNF to request rebuild confirmation (based on the package hash from repository metadata) before actually downloading package. There is still some work to do on this plugin, but the basic functionality is there already:

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

As explained above, some parts still need finishing, as well as cleanups and proper documentation. But we are very close to the point where every Debian package we build can be independently verified. The very same tooling we've made can be used to verify native Debian packages too, which should also be helpful for non-Qubes Debian users.
Similar progress is already made for Fedora, although some more work is needed on Fedora side to allow reproducing native (not only Qubes-related) packages.


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
[rpmreproduce]: https://github.com/fepitre/rpmreproduce
[rpm buildinfo]: https://github.com/rpm-software-management/rpm/pull/1532
[dnf-plugin-in-toto]: https://github.com/fepitre/dnf-plugin-in-toto
[moss-award]: /news/2020/05/22/moss-mission-partners-award/
[Mozilla Open Source Support (MOSS)]: https://www.mozilla.org/en-US/moss/
