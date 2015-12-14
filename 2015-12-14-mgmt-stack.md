---
layout: post
title: Salt management stack in Qubes
date: 2015-12-14
categories:
 - articles
author: Marek Marczykowski-Górecki
---

With Release 3.1, we are introducing a management stack. Its main purpose is to
provide an easy and automated way to setup even the most complex configurations within Qubes.
For example, installing and configuring Whonix on Qubes requires installation of
two templates, creating at least two VMs, setting some properties. Generally a
lot of manual work - [documented well][whonix-install-doc], but still not very
user friendly. The same goes for some other use cases like USB VM, Split GPG
and many more.

We have decided to base Qubes management stack on an existing solution. The
reason for that is quite simple: to not reinvent the wheel. There are already
multiple mature projects implementing system management, so we have decided to
source from their experience. The most important feature we sought in existing 
solutions was declarative configurations: being able to describe desired system 
This is a huge gain over imperative approaches (like custom scripts), which would 
need to handle all corner cases, all possible interactions, and would soon get
really hard to maintain.

From all the available projects, we have chosen [Salt Stack][saltstack], because 
of its wide adoption, ability to run without any network service ("local" mode), 
clean and easy to read configuration, and ease of extensibility (python API for modules). 
In addition, it has the ability to manage (possibly remote) system which doesn't 
have Salt stack agent ("minion"), a mechanism known as `salt-ssh`
(which in fact isn't that unique to Salt Stack). With Salt Stack, we have implemented a
module to handle Qubes configuration, specifically managing VMs.

The current implementation released in Qubes 3.1 (rc1) is limited to only
manage VMs at dom0 level (create, set properties etc). Managing configuration
inside VMs (like installing additional packages there) is not yet available.
Also the current release doesn't allow setting global properties like default
template or default netvm. Those features are planned, and to some degree
already implemented, but not mature enough yet to be included in Qubes 3.1
release.

In R3.1 rc1 we have included configuration to setup:
 * [Whonix][whonix-template] VM (both gateway and workstation) - just one click!
 * USB VM, including [input proxy][input-proxy] configuration for USB mouse!
 * Basic VMs creation (sys-net, sys-firewall, personal, vault, etc)

But in the near future we are planning more, for example:
 * Split GPG setup
 * Streamline system updates (dom0 and all the templates)

Then, based on this functionality, we will be able to create a Management VM,
which will allow secure, centralized management of Qubes OS installations
in an organization or company. But to do it securely, we need to first finish
some major rework of Qubes core management code ("core3"), which is planned
for Qubes 4.0. Then it will be possible to implement Management VM in a way so that it
will have no access to user data, only ability to manage configuration of
(selected) VMs.

Going back to the present time - if you want to read more about technical
details of the current implementation and configuration, take a look at the
[documentation][salt-doc].

And finally, some example configurations, taken from [default set][salt-configs] -
configuration of Whonix-Gateway:

~~~
include:
  - qvm.template-whonix-gw
  - qvm.sys-firewall

{%- from "qvm/template.jinja" import load -%}

# Create sys-whonix ProxyVM
{% load_yaml as defaults -%}
name:          sys-whonix
present:
  - template:  whonix-gw
  - label:     purple
  - mem:       500
  - flags:
    - proxy
prefs:
  - netvm:     sys-firewall
  - autostart: true
require:
  - pkg:       template-whonix-gw
  - qvm:       sys-firewall
{%- endload %}

{{ load(defaults) }}

# Set sys-whonix as netvm for whonix-gw template
{% load_yaml as template -%}
name:          whonix-gw
force:         true
prefs:
  - netvm:     sys-whonix
require:
  - pkg:       template-whonix-gw
  - qvm:       sys-whonix
{%- endload %}

{{ load(template) }}
~~~

Enjoy!

[whonix-install-doc]: https://www.qubes-os.org/doc/privacy/install-whonix/
[saltstack]: https://saltstack.com/
[whonix-template]: https://www.qubes-os.org/doc/templates/whonix/
[input-proxy]: https://github.com/qubesos/qubes-app-linux-input-proxy
[salt-doc]: https://www.qubes-os.org/doc/salt/
[salt-configs]: https://github.com/QubesOS/qubes-mgmt-salt-dom0-virtual-machines
