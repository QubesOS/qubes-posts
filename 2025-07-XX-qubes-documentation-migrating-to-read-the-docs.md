---
layout: post
title: "The Qubes documentation is migrating to Read the Docs!"
categories: announcements
---

We're pleased to announce that we're officially migrating to [Read the Docs](https://readthedocs.com/) as our documentation generation and hosting platform. Our documentation source files will continue to reside in the [qubes-doc](https://github.com/QubesOS/qubes-doc) Git repository with [PGP-signed tags and commits](/security/verifying-signatures/#how-to-verify-signatures-on-git-repository-tags-and-commits), and the live documentation published on the web will continue to be located on the official Qubes website, but Read the Docs will handle generating the documentation from our source files and hosting the generated documentation on the backend so that it can be served to Qubes website visitors. Migrating to Read the Docs will enable us to localize the documentation, maintain release-specific documentation, support offline documentation, and more. Today marks the beginning of a 20-day community testing period for the new documentation, which is already live at <https://doc.qubes-os.org/en/latest/>.

## What is Read the Docs?

[Read the Docs](https://readthedocs.com/) is an open-source platform that hosts software documentation. It's a popular choice among open-source projects,  because it simplifies the process of generating documentation from Git repositories and offers many useful features that would be difficult for small projects to implement themselves in a polished way, such as versioning, integrated search, and pull request previews.

## Why is the documentation migrating?

There are new and increasing demands on the Qubes documentation, such as localization (i.e., translations), release-specific documentation, and offline documentation. With these new demands, it has become increasingly difficult for the current system to meet our needs. Before we tried to address these kinds of needs, our documentation setup was simpler and served us well. The backend was smaller, easier to understand, and easier to maintain, since it didn't need to do much. There was only a single, canonical version of every documentation page.

However, the reality is that not all Qubes users read English, not everyone uses the same version of Qubes OS at the same time, and not everyone is comfortable cloning a Git repo and sorting through plain text files written in a lightweight markup language. We have users and contributors of all technical levels from around the world, and we sometimes support multiple Qubes releases simultaneously. Even when there's only one currently-supported release, there's usually a new one in development or testing that folks want to start documenting. This means that we have to support a separate version of each relevant documentation page for every translated language and for every supported and in-development Qubes version, not to mention historical documentation for releases that have reached end of life (EOL). We also have users who want to be able to access the documentation offline in a user-friendly format, such as HTML, PDF, or EPUB.

Trying to implement all of these features and manage all of this complexity manually by ourselves isn't practical. We need a system that's designed for managing complex software documentation, and that's precisely what Read the Docs is.

## How is the documentation currently set up, and what will change?

At present, the official Qubes documentation consists of a set of plain text [Markdown](https://en.wikipedia.org/wiki/Markdown) source files that are stored in the [qubes-doc](https://github.com/QubesOS/qubes-doc) Git repository, which is a subrepository of the Qubes website repository, [qubesos.github.io](https://github.com/QubesOS/qubesos.github.io). We then use [GitHub Pages](https://pages.github.com/) to generate the [Qubes website](/), which includes the [current documentation](/doc/).

Read the Docs also generates documentation from plain text source files in Git repositories. However, it primarily uses [reStructuredText](https://en.wikipedia.org/wiki/ReStructuredText) instead of Markdown. Furthermore, Read the Docs hosts HTML content that is usually generated using tools like [Sphinx](https://www.sphinx-doc.org/), whereas GitHub Pages uses [Jekyll](https://jekyllrb.com/) for static site generation.

Accordingly, the Qubes documentation source files will continue to reside in the [qubes-doc](https://github.com/QubesOS/qubes-doc) repository, but the Markdown content is being converted to reStructuredText. Furthermore, the documentation will be generated using Sphinx and hosted by Read the Docs rather than generated using Jekyll and hosted by GitHub. The web version of the new documentation will continue to be located on the Qubes website, but it will be under a new subdomain (<https://doc.qubes-os.org>). In addition, images and other binary files used in the documentation will be stored in [qubes-doc](https://github.com/QubesOS/qubes-doc) rather than the [qubes-attachment](https://github.com/QubesOS/qubes-attachment) repository.

## How will this affect the security of the documentation?

The source files for all official Qubes documentation will continue to be stored in the [qubes-doc](https://github.com/QubesOS/qubes-doc) Git repository with [PGP-signed tags and commits](/security/verifying-signatures/#how-to-verify-signatures-on-git-repository-tags-and-commits), just as they are now. In that sense, the security of the documentation won't change at all. The main change is that Read the Docs will replace GitHub as the platform that generates the documentation from the source files and hosts the generated documentation.

## How will the migration proceed?

Over the next 20 days, between now and <TODO_YYYY-MM-DD>, the community will test the new documentation. During this period, the current documentation will be frozen, which means that no pull requests will be merged. After the testing period has concluded, the documentation team will evaluate the results and make any changes that are needed. Once the migration is complete, we'll make a final announcement, and the new documentation hosted on Read the Docs will officially replace the current documentation.

## What can I do to help?

Please review the new documentation located at <https://doc.qubes-os.org/en/latest/>. It would be especially helpful to compare the new documentation to the current documentation located at <https://qubes-os.org/doc> and point out any inconsistencies. (When doing this, please focus on accuracy and correctness rather than visual aspects.)

There's a full list of new documentation pages to be reviewed in issue [#8180](https://github.com/QubesOS/qubes-issues/issues/8180). After you've reviewed a page, please leave a comment on that issue to let us know your findings. You can simply copy the entries you reviewed from that list into your comment and add a brief note on what you found problematic, found acceptable, and so on. If you'd like to report a more significant bug or request a feature, you can also [open a separate issue](/doc/issue-tracking/). If you're familiar with reStructuredText and would like to submit a change to the new documentation, please open a pull request against this branch: <https://github.com/QubesOS/qubes-doc/tree/rst>.

While we want to know about any problems you spot, please feel free to share any positive feedback you have, as well!

## Acknowledgments

This migration is the culmination of many hours of work over the course of several years, for which the Qubes OS Project owes a debt of gratitude. The Sphinx conversion tooling was done by [Maiska (aka m)](/team/#m), with support from [Marek Marczykowski-Górecki](/team/#marek-marczykowski-górecki). [Tobias Killer (aka tokidev)](/team/#tobias-killer) thoroughly and repeatedly tested the converted and deployed reStructuredText documentation. More recently, [unman](/team/#unman) and [Solène Rapenne](/team/#solène-rapenne) have supported the transition in the capacities as documentation maintainers.
