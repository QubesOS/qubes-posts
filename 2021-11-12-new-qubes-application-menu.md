---
layout: post
title: "New Qubes application menu"
categories: articles
author: Marta Marczykowska-Górecka
---

## The new application menu is here!

If you are running a release candidate of [Qubes OS 4.1](/news/2021/10/11/qubes-4-1-rc1/) and wish to just dive on in, the new app menu can be found [here](https://github.com/QubesOS/qubes-desktop-linux-menu). But first, a couple important caveats:

- **This new menu requires 4.1 and cannot run on 4.0.**
- **This is experimental software for testing purposes only!**

Still want to give it a go? To install, enter this command in a dom0 terminal:

```
sudo qubes-dom0-update --enablerepo=qubes-dom0-unstable qubes-desktop-linux-menu
```

Once installed, add the new menu to the XFCE panel using the provided `.desktop` file ([instructions](https://github.com/QubesOS/qubes-desktop-linux-menu#how-to-run)). For those who want to learn more, read on!


## Background

One of the key issues raised by users in last year's Qubes User Survey (see the [write-up](https://www.qubes-os.org/news/2020/11/26/qubes-survey-results/) --- and a big thank you to everyone who participated!) was general usability. Qubes OS is great for security, but the *experience* of using it can be somewhat opaque or even confusing. The UX difficulties are exacerbated by the fact that most of our GUI components are adapted from those designed for single-environment systems, like XFCE on Fedora. This served as a good first pass for an open-source project developed by a small team, but the time has come to begin working on a GUI tailored to Qubes OS's particular multi-environment setting. 

Helpfully, three years ago the SecureDrop team began user research in support of their [SecureDrop Workstation project](https://securedrop.org/news/piloting-securedrop-workstation-qubes-os/) (built atop Qubes OS). In the course of their work, they discovered that it was not just SecureDrop users who wanted the Qubes OS GUI to be friendlier --- a lot of other folks seemed to want Qubes OS to be more usable, too! From the SecureDrop Workstation project, Nina Alter began contributing to Qubes, and we subsequently joined forces to tackle the app menu project. This project received a generous grant from the Mozilla Foundation!


## Goals

The main idea behind the new application menu is simple: to create a way of accessing programs that's native to Qubes OS, takes into account its quirks and approach, and is both easy to use and accessible. The visual clarity of the current application menu, which uses XFCE's default menu and adds a folder within the menu for each qube, leaves much to be desired. Research showed us most users prefer GUI system tools. The classic Linux nerd answer of "Just use the terminal. It's easy!" does not really capture how most people work. Thus, we needed a better approach, one that's more accessible, easier to use, and represents a mental model consistent with Qubes OS rather than a typical monolithic Linux distribution.

For those interested, [we presented our work on the new app menu on the second day of our 2021 Qubes Mini Summit](https://www.youtube.com/watch?v=KdDr6TiqF0k). The presentation begins at the 01h 15min mark of the video.


## Application Menu Features

### Qubes

The new application menu has three tabs (as seen on the left): qubes, favorites, and system tools. The first tab is the most similar one to the old menu. It contains all qubes, but now sorted into three groups (on top of the middle pane): normal application qubes (the APP section), qube templates (TEMPLATES), and various system qubes (SYSTEM).

[![Menu](/attachment/posts/menu_writeup_1.png)](/attachment/posts/menu_writeup_1.png)

When you select a qube, its applications appear on the right --- the same applications that you chose for the old menu, in Qube Settings.  Now, however, they are accompanied by a couple of utility features, like quick access to start and shut down commands and an indicator of the networking state of the qube on top.

The menu itself also communicates more information about system state. Names of running qubes are bolded, and those of disposable qubes and their templates are italicized. There's also a clear visual indicator of the disposable template each disposable is based on.
Further enhancements (coming in the future) will be --- as inspired by many users citing frustrations with memory management and information in the menu --- more data about RAM usage or qube template on top of the application pane.

[![Disposable qube indicator](/attachment/posts/menu_writeup_2.png)](/attachment/posts/menu_writeup_2.png)

In order to make the use of disposable qubes more conveniently, now programs can be started in a running disposable qube from the menu. It works just like any other qube. There's only one limitation: If the qube was started for a program (which is usually the case), it will shut down when that first program is closed. 


### Favorites

You asked, and we have delivered! Second in the primary menu, after qubes, is a completely new tab: Favorites.

[![Favorites](/attachment/posts/menu_writeup_3.png)](/attachment/posts/menu_writeup_3.png)

You can right-click on any application in the qube menu and add it to your favorites. It will then appear in this menu. To remove it, simply right-click on the app within the Favorites tab and select "Remove from Favorites."


### System tools

The last tab is devoted to all sorts of configuration and system tools and, in practice, also "random things installed in dom0." (It's a bad idea to install random things in dom0, but if it happens, that's where you will find them.) System tools can also be added to favorites. Some of us find it useful, for example, to have a dom0 terminal shortcut there.

[![System tools](/attachment/posts/menu_writeup_4.png)](/attachment/posts/menu_writeup_4.png)


### Installing and running the application menu

As the menu is not yet part of Qubes by default, you have to install it yourself with:

```
[user@dom0 ~]$ sudo qubes-dom0-update --enablerepo=qubes-dom0-unstable qubes-desktop-linux-menu
```

The menu can then be added to the XFCE panel as a widget, with Panel -> Add New Items -> Launcher, and in the Launcher add the Open Qubes Application Menu option. Normally, the process that provides the menu with data will be running in the background, but it will only start on the next reboot. To avoid the need for a reboot, you can just run the following in a dom0 terminal:

```
[user@dom0 ~]$ qubes-app-menu &
```

There are also several helpful options in the command line for users who want more customization, like `--keep-visible` (if you want the menu to be always visible) or `--page [N]` to select the page at which the menu should be opened (`0` for apps, `1` for favorites, and `2` for system tools). The entire option list is available, as usual, through `qubes-app-menu --help`.


### Feedback and testing

As this is very much a first release, bugs are likely. Please [report any issues you discover](/doc/issue-tracking/).

We'd also very much welcome anonymous feedback on the new menu through our [survey tool](https://survey.qubes-os.org/index.php?r=survey/index&sid=255277&lang=en).

The current plan is to have this menu become the default in Qubes 4.2, but of course compatibility with other menus will be maintained. Our current development status can be seen in the [GitHub project for the new application menu](https://github.com/QubesOS/qubes-issues/projects/12).

Enjoy!

