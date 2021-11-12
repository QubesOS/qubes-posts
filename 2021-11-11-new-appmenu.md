---
layout: post
title: "New Qubes Application Menu"
categories: articles
author: Marta Marczykowska-Górecka
---


# The New Application Menu is here!


If you are running a release candidate of [Qubes OS 4.1](#) and wish to just dive on in, the new app menu can be found [here](https://github.com/QubesOS/qubes-desktop-linux-menu). _The new menu requires 4.1, and cannot run on 4.0._

To install, use the below in a dom0 terminal:
```
sudo qubes-dom0-update --enablerepo=qubes-dom0-unstable qubes-desktop-linux-menu
```
Once installed, add the new menu to the XFCE panel using the provided .desktop file ([instructions](https://github.com/QubesOS/qubes-desktop-linux-menu#how-to-run)). For those who want to learn more, read on!

## Background

One of the key issues raised by users in last years’ Qubes User Survey (see [writeup](https://www.qubes-os.org/news/2020/11/26/qubes-survey-results/) - and a big thank you to everyone who participated!) was general usability. Qubes OS is great and reasonably secure and all, but the *experience* of using it can be somewhat opaque or even confusing. The UX difficulties are exarcebated by how most of todays Qubes OS GUI comes from slightly-modified components of single-environment systems like Fedora/XFCE. It served as a good first GUI for an open-source project developed by a small team, but the time has come to begin working on GUI tailored to Qubes OS's particular multi-environment setting. 

Helpfully, three years ago the SecureDrop team began user research in support of their [SecureDrop Workstation project](https://securedrop.org/news/piloting-securedrop-workstation-qubes-os/) (built atop Qubes OS). During their work, it was discovered that not just SecureDrop users wanted the Qubes OS GUI to be friendlier - a lot of other folks seem to want Qubes OS to be more usable, too! From the Workstation project, Nina Alter began contributing to Qubes, and later took on the app menu project with Marta Marczykowska-Górecka. This project received a generous grant from Mozilla Foundation. 


## Goals

The main idea behind the new Application Menu was simple: let’s create a way of accessing programs that’s native to Qubes OS, takes into account its quirks and approach, and is both easy to use and acccessible. The visual clarity of the current Application Menu - which uses XFCE’s default menu and adds a folder within the menu for each VM - can be very lacking. Research showed us most users use GUI system tools - the classical Linux nerd answer of “just use a terminal, it’s easy” it’s not really how most people work. Thus we needed a better approach - and one that would be more accessible, easier to use and show a mental model consistent with Qubes OS, not with a typical Linux distribution.

For those interested, [we presented our work on the new app menu on the 2nd day of 2021's Qubes Mini Summit](https://www.youtube.com/watch?v=KdDr6TiqF0k). The presentation begins at the 01h 15min mark of the video.


## Application Menu Features

### Qubes

The new Application Menu has three tabs (as seen on the left) - qubes, favorites and system tools. The first tab is the most similar one to the old menu - it contains all qubes, but now sorted into three groups (on top of the middle pane): normal application qubes (the APP section), qube templates (TEMPLATES) and various system qubes (SYSTEM).

[![Menu](/attachment/posts/menu_writeup_1.png)](/attachment/posts/menu_writeup_1.png)

When you select a qube, its applications appear on the right - the same applications that you chose for the old menu, in Qube Settings.  Now, however, they are accompanied by a couple of utility features, like quick access to start and shut down commands, and an indicator of the networking state of the qube on top.

The menu itself is also communicating more information about system state - names of running qubes are bolded, and those of disposable qubes and their templates are italicized. There's also a clear visual indicator of the disposable qube template each disposable qube is based on.
Further enhancements (coming in the future) will be - as inspired by many users citing frustrations with memory management and information in the menu - more data about RAM usage or qube template on top of the application pane.

[![Disposable qube indicator](/attachment/posts/menu_writeup_2.png)](/attachment/posts/menu_writeup_2.png)

In order to make the use of disposable qubes more convenient, now programs can be started in a running disposable qube from the menu - it appears in it just like any other qube. There's only one limitation - if the qube was started for a program (which is usually the case), it will shut down when that first program is closed. 


### Favorites

You asked, we have delivered! Second in the primary menu after qubes, is a completely new tab: Favorites.

[![Favorites](/attachment/posts/menu_writeup_3.png)](/attachment/posts/menu_writeup_3.png)

You can right-click on any application in the qube menu and add it to favorites - it will then appear in this menu. right-click on the app within the Favorites tab, and select "Remove from Favorites."

### System tools

The last tab is devoted to all sorts of configuration and system tools - and, in practice, also "random things installed in dom0" (it's a bad idea to install random things in dom0, but if it happens, that's where you will find them). System tools can also be added to favorites - some of us find it, for example, to have dom0 Terminal there.

[![System tools](/attachment/posts/menu_writeup_4.png)](/attachment/posts/menu_writeup_4.png)


### Installing and running the Application Menu

As the menu is not yet part of Qubes default, you have to install it yourself with:
```
[user@dom0 ~]$ sudo qubes-dom0-update --enablerepo=qubes-dom0-unstable qubes-desktop-linux-menu
```
The menu can then be added to XFCE panel as a widget, with Panel -> Add New Items -> Launcher, and in the Launcher add the Open Qubes Application Menu option. Normally, the process that provides menu with data will be running in the background, but it will only start on the next reboot. To avoid the need for a reboot, you can just run the following in dom0 Terminal:
```
[user@dom0 ~]$ qubes-app-menu &
```
There are also several helpful options in the command line for users who want more customization, like `--keep-visible` (if you want the menu to be always visible) or `--page [N]` to select which page (apps - 0, favorites - 1 and system tools - 2) the menu should be opened at). The entire option list is available, as usual, through `qubes-app-menu --help`

### Feedback and testing

As this is very much a first release, bugs are likely. Please report any issues in our [GitHub issues repository](https://github.com/QubesOS/qubes-issues/issues) or in the [forum](https://forum.qubes-os.org/). 

We'd also very much welcome anonymous [feedback on the new menu through our survey tool](https://survey.qubes-os.org/index.php?r=survey/index&sid=255277&lang=en).

The current plan is to have this menu become a default in 4.2 - but of course compatibility with other menus will be maintained. The current development can be seen in the [GitHub project for the new Application Menu](https://github.com/QubesOS/qubes-issues/projects/12).

Enjoy!

