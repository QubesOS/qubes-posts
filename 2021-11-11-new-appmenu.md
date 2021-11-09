---
layout: post
title: "New Qubes Application Menu"
categories: articles
author: Marta Marczykowska-Górecka
---


# New Application Menu

One of the big issues raised by users in last years’ Qubes User Survey (see [writeup](https://www.qubes-os.org/news/2020/11/26/qubes-survey-results/) - and a big thank you to everyone who participated!) was general usability. Qubes OS is great and all, but can be somewhat opaque - and it’s not helped by the GUI being mostly slightly-modified tools from single-environment systems like XFCE. One of our attempts at improving this situation - generously funded by Mozilla and Freedom of the Press Foundation - is creating a new and improved Application Menu.

If you don’t want to read too many words, the new menu requires Qubes 4.1, you can find it [here](https://github.com/QubesOS/qubes-desktop-linux-menu), install it like this:
```
sudo qubes-dom0-update --enablerepo=qubes-dom0-unstable qubes-desktop-linux-menu
```
and add it to the XFCE panel using the provided .desktop file ([instructions](https://github.com/QubesOS/qubes-desktop-linux-menu#how-to-run)). For those who want to learn more, read on!

## Goals

The main idea behind the new Application Menu was simple: let’s create a way of accessing programs that’s native to Qubes OS, takes into account its quirks and approach, and is both easy to use and acccessible. The visual clarity (or lack of it) of the current Application Menu - which uses XFCE’s default menu and adds a folder within the menu for each VM - can be very user-unfriendly. Research showed us most users use GUI system tools - the classical Linux nerd answer of “just use a terminal, it’s easy” it’s not really how most people work. Thus we needed a better approach - and one that would be more accessible, easier to use and show a mental model consistent with Qubes OS, not with a typical Linux distribution.

## Features

### Qubes

The new Application Menu has three tabs - qubes, favorites and system tools. The first tab is the most similar one to the old menu - it contains all qubes, but now sorted into three types: normal application qubes (the APP section), qube templates (TEMPLATES) and various system qubes (SYSTEM).

[![Menu](/attachment/posts/menu_writeup_1.png)](/attachment/posts/menu_writeup_1.png)

When you select a qube, its applications appear on the right (the same ones that you chose for the old menu, in Qube Settings) - but this time, they are accompanied by a couple of utility features, like quick access to Start/Shutdown Qube commands, and an indicator of the networking state of the qube on top.
The menu itself is also communicating more information about qube state - names of running qubes are bolded, and those of disposable qubes and their templates are italicized. There's also a clear visual indicator of the disposable qube template each disposable qube is based on.

[![Disposable qube indicator](/attachment/posts/menu_writeup_2.png)](/attachment/posts/menu_writeup_2.png)

In order to make the use of disposable qubes more convenient, now you can start more programs in a running disposable qube from the menu - it appears in it just like any other qube. There's only one limitation - if the qube was started for a program (which is usually the case), it will shut down when that first program is closed. 

### Favorites

A completely new tab is the Favorites tab.

[![Favorites](/attachment/posts/menu_writeup_3.png)](/attachment/posts/menu_writeup_3.png)

You can right-click on any application in the qube menu and add it to favorites - it will then appear in this menu. To remove it, right-click in the Favorites tab on the app, and a "remove from favorites" option will appear. 

### System tools

The last tab is devoted to all sorts of configuration and system tools - and, in practice, also "random things installed in dom0" (it's a bad idea to install random things in dom0, but if it happens, that's where you will find them). System tools can also be added to favorites - I personally find it very useful to have a dom0 Terminal there.

[![System tools](/attachment/posts/menu_writeup_4.png)](/attachment/posts/menu_writeup_4.png)

### Installing and running the Application Menu

As the menu is not yet part of Qubes default, you have to install it yourself with:
```
[user@dom0 ~]$ sudo qubes-dom0-update --enablerepo=qubes-dom0-unstable qubes-desktop-linux-menu
```
The menu can then be added to XFCE panel as a widget, with Panel -> Add New Items -> Launcher, and in the Launcher add the Open Qubes Application Menu option. Normally, the process that provides menu with data will be running in the background, but it will start on the next reboot. To avoid the need for a reboot, you can just run the following in dom0 Terminal:
```
[user@dom0 ~]$ qubes-app-menu &
```
There are also several helpful options in the command line for users who want more customization, like `--keep-visible` (if you want the menu to be always visible) or `--page [N]` to select which page (apps - 0, favorites - 1 and system tools - 2) the menu should be opened at). The entire option list is available, as usual, through `qubes-app-menu --help`

### Feedback and testing

As this is very much a first release, bugs are likely. Please report any issues in our [GitHub issues repository](https://github.com/QubesOS/qubes-issues/issues) or in the [forum](https://forum.qubes-os.org/). The current plan is to have this menu become a default in 4.2 - but of course compatibility with other menus will be maintained.

Enjoy!

