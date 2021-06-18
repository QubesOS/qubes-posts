---
layout: post
title: "Qubes Architecture Next Steps: The New Qrexec Policy System"
categories: articles
author: Marek Marczykowski-Górecki, Marta Marczykowska-Górecka
---

This is the second article in the "What's new in Qubes 4.1?" series. You
can find the previous one (about GUI Domains)
[here](/news/2020/03/18/gui-domain/). While the
introduction of GUI domains is a big, singular feature, the changes to
qrexec are more complex and varied --- but also very important.

## What is qrexec?

You might have been using Qubes for a while and never encountered the
word "qrexec," but it is one of the crucial components in the system.
Qubes provides isolation and compartmentalization. Separate qubes are
separate worlds, and what the user does in one qube should not impact
another qube. That is, of course, not realistic. In the real world, we
often need and want to do things like copying and pasting, sending
files, routing internet traffic, and simply synchronizing the time. This
is where qrexec comes in: It is an RPC (remote procedure call) mechanism
that allows one qube to do something inside another qube. Of course,
allowing everything all the time would be extremely dangerous. Thus, a
part of qrexec called "qrexec policy" is also used to enforce who can do
what and where. Furthermore, we want to be able to audit what was done,
and this is provided with the logging capabilities of qrexec. The *how*
is controlled by qrexec services, which are executed by qrexec and also
must be designed in a secure and resilient way. This post focuses mainly
on qrexec itself, but qrexec services will make a brief appearance.

## Overview of changes

*Most of this post will be very technical. If you don't care about
writing your own qrexec policies and services, feel free to just read
this overview.*

Before we get into nitty-gritty technical details, here's a brief, less
technical overview of Qubes 4.1 qrexec changes and what they mean for
users and developers:

 - A new qrexec policy format. (The old format is still supported, but
   the new one is very much superior, allowing for easier-to-read
   policies, more qube-centered customization, better auditing, and
   more.)
 - Big performance improvements: bigger data chunk sizes for faster
   transfers of large amounts of data, qrexec policy daemon for faster
   policy evaluation and call setup, resulting in up to seven-fold
   faster qrexec service calls.
 - Support for socket services: better performance for services that can
   use a socket-based implementation with significantly faster setup and
   connection times.
 - Policy notifications that make any abnormal behavior easier to detect
   and any problems resulting from incorrect permissions easier to
   solve.

There is one other big upcoming change (which is not yet fully
implemented and will arrive after Qubes 4.1): We are working on a qrexec
policy API, that is, a set of qrexec services that will allow for
managing qrexec policies without manually editing policy files. It's
another step toward separating the user from dom0 and protecting the
vulnerable system internals by isolating them from the outside world to
the greatest extent possible.

## New policy format

In Qubes 4.0 and earlier, policies were stored as multiple files, one
for each service. While changing permissions for a single action was
easy, managing permissions for an entire qube was very cumbersome. The
new Qubes 4.1 policy format completely overhauls this approach and
introduces several convenience features to make policy management easier
and more secure. 

What if you've already spent time carefully crafting your policies in
Qubes 4.0? Don't worry. The old policy format will still be supported
until at least Qubes 5.0.

You can find details of the development of the new policy format
[here](https://github.com/QubesOS/qubes-issues/issues/4370).

### Policy files

The biggest difference between the old and new policy formats is that,
under the new format, the entire policy is a single entity. It is not
divided into separate, per-service fragments. While it can be stored in
multiple files located in multiple places, the files are equivalent, and
each can describe policies for multiple services. In other words, we are
moving from a set of tables to one big, flat table.

The policy files, stored previously in `/etc/qubes-rpc/policy/`, are now
located in `/etc/qubes/policy.d/`. Furthermore, each file must have a
`.policy` extension, and any temporary files will no longer cause
issues. Under the old format, there were dozens files:

```
/etc/qubes-rpc/policy/
├── admin.backup.Cancel
├── admin.backup.Execute
├── admin.backup.Info
├── include
│   ├── admin-local-ro
│   ├── admin-local-rwx
│   ├── admin-global-ro
│   ├── admin-global-rwx
├── …
├── qubes.Gpg
├── qubes.StartApp
├── …
├── whonix.SdwdateStatus
```

Under the new format, we simply have:

```
/etc/qubes/policy.d/
├── 35-compat.policy
├── 90-admin-default.policy
├── 90-default.policy
├── include
│   ├── admin-local-ro
│   ├── admin-local-rwx
│   ├── admin-global-ro
│   ├── admin-global-rwx
(and any additional user-defined files)
```

Here's an example of an old policy file (`qubes.GetDate.policy`):

```
## Note that policy parsing stops at the first match,
## so adding anything below "$anyvm $anyvm action" line will have no effect

## Please use a single # to start your custom comments

$tag:anon-vm	$anyvm	deny
$anyvm	$anyvm	allow,target=dom0
```

Here's an example of the beginning of a new policy file
(`90-default.policy`):

```
## Do not modify this file, create a new policy file with lower number in the
## filename instead. For example `30-user.policy`.

###
### Default qrexec policy
###

## File format:
## service-name|*       +argument|* source          destination action  [options]

## Note that policy parsing stops at the first match.

# policy.RegisterArgument should be allowed only for specific arguments.
policy.RegisterArgument *           @anyvm          dom0        deny

# WARNING: The qubes.ConnectTCP service is dangerous and allows any
# qube to access any other qube TCP port. It should be restricted
# only to restricted qubes. This is why the default policy is 'deny'

# Example of policy: qubes.ConnectTCP +22 mytcp-client @default allow,target=mytcp-server
qubes.ConnectTCP        *           @anyvm          @anyvm      deny

# VM advertise its supported features
qubes.FeaturesRequest   *           @anyvm	        dom0	    allow

# Windows VM advertise installed Qubes Windows Tools
qubes.NotifyTools       *           @anyvm          dom0        allow

# File copy/move
qubes.Filecopy          *           @anyvm          @anyvm      ask

# Get current date/time
qubes.GetDate           *           @tag:anon-vm    @anyvm      deny
qubes.GetDate           *           @anyvm          @anyvm      allow target=dom0
```

Under the new format, the policy for a single qube is much easier to
find, parse, and edit. It's all contained in a single file, and by using
wildcards (see below), it's much easier to read and understand. For
example, it's trivial now to forbid all calls for a given qube or forbid
all with a list of exceptions. This allows for a more qube-centric
approach to policies, and while the results are theoretically the same,
there's a huge difference between a couple of rules, all in one places,
and dozens of rules spread across dozens of files. It's especially
important for Admin API policies. (The Admin API provides qrexec calls
for querying system state. There are a lot of them, and they are needed
by system qubes such as GUI domains. Under the old policy format, it was
quite arduous to edit them when each call had its own policy file.)

Now that the policy is a single entity, it is parsed as a whole. If
there are any syntax errors, the parser will refuse to load anything (in
order to prevent any unintended permission grants). The system is
designed to "failed closed": An empty policy results in all qrexec calls
being denied. While this protects users from their own mistakes, users
should still be careful when editing policies so that the system remains
functional. This is similar to the approach that tools such as `sudo`
take, and we decided that it makes for a safer system on the whole. In
the future, Qubes will also include a Policy API (see below) that will
include calls for modifying the policy in a safe way that prevents
syntax errors.

### New column syntax

*The full specification can be found
[here](https://github.com/QubesOS/qubes-core-qrexec/blob/master/Documentation/multifile-policy.markdown).*

Old policy files had three columns: source qube, target qube, and action
(allow/deny/ask with optional arguments). Because the new policy format
no longer includes the service name in the file name, we had to change
this a bit to a five-column format: service name, service argument,
source qube, target qube, and action.

```
qrexec.Service +ARGUMENT SRCQUBE DSTQUBE {allow|deny|ask} [PARAM=VALUE [PARAM=VALUE ...]]
```

Under the old format, you could use wildcards only in the source qube
and target qube fields. Under the new format, you can also use wildcards
in the service name and argument fields. For example, to forbid all
calls from the `work` qube to the `personal` qube, you could use this
line:

```
* * work personal deny
```

You could also deny all calls from `work` except for calls to `work-pub`:

```
* * work work-pub allow
* * work * deny
```

This, along with the new file structure, results in much clearer,
qube-centered policy rules.

A minor change is the replacement of commas with spaces in the parameter
list. For example, a ConnectTCP policy could look like this:

```
qubes.ConnectTCP * mytcp-client @default allow target=mytcp-server
```

### Security in symbols

The old policy format used the `$` character in multiple keywords, such
as `$default` and `$dispvm`. Due to a potential issue described in
[Qubes Security Bulletin #38](/news/2018/02/20/qsb-38/),
we have been getting rid of `$`. (In brief, interaction between Bash
shell parameter expansion and `$` characters in policy files could
theoretically lead to security problems.) For backward compatibility,
`$` is still supported in policy files, but it is being internally
turned into `@` before processing, and in the new policy format, we only
use `@`. In other words, `$default` in Qubes 4.0 will now be `@default`,
`$dispvm` turns into `@dispvm`, and so on. For example, to update all
TemplateVMs through `sys-whonix`, you would use:

```
qubes.UpdatesProxy * @type:TemplateVM @default allow target=sys-whonix
```

In all client tools, `$` will still be automatically translated into
`@`, so you don't have to change any existing scripts. However, we
highly recommend using only `@`. Legacy support for `$` will continue
throughout the Qubes 4.x series and end in Qubes 5.0.

### Includes

The old format supported a simple `$include` directive. In Qubes 4.1,
the inclusion capabilities are extended, and we now have four different
kinds of includes. Due to the aforementioned QSB #38 change, the
keywords no longer start with `$`, but rather with an exclamation point
(`!`). The supported include directives are:

 - `!include path_to_file` --- The simplest command. Includes the entire
   contents of the file and parses it according to new policy syntax
   rules. The file can contain policy rules for one or more services,
   for one or more qubes.
 - `!include-dir path_to_directory` --- Includes all `.policy` files
   from the directory, parsing them according to new policy syntax
   rules.
 - `!include-service service argument path_to_file` --- Allows for the
   inclusion of a policy in a format similar to the old one (without a
   service or an argument) by filling in the missing service and
   argument fields from the new format with parameters provided to
   `include-service`. Either of those (or both) can be a wildcard (`*`),
   as they can be in all new policy files. (This might seem like a
   directive useful only for backward compatibility, but it also has
   uses in the Admin API policies used for implementing GUI domains,
   among other things.)
 - `!compat-4.0` --- An explicit backward compatibility directive that
   takes the old policy files from their old locations and converts them
   internally (using `include-service`) to be a part of the new policy.

### Backward compatibility

One of the new policy files is `35-compat.policy`. It loads the old
policy files and parses them in a way compatible with the new policy
format.

Before the old policy format is completely phased out in Qubes 5.0, we
will provide a conversion tool. (This tool may not be available in Qubes
4.1 on release.) Since there will be legacy support for the old policy
format with `35-compat.policy` and the `!compat-4.0` include directive,
it is not necessary for users to update their current custom policies
for Qubes 4.1.

## Performance improvements

In Qubes 4.1, the max qrexec data chunk size has been increased from 4kB
to 64kB. This makes large data transfers significantly faster, for
example, copying big files or streaming video through `qvm-usb` from
`sys-usb` to a qube with an ongoing video call. (Details can be found
[here](https://github.com/QubesOS/qubes-issues/issues/4909).)

We have also significantly increased the maximum service and argument
length. In Qubes 4.0, it was just 63 characters for the combined
service+arguments. Now, it is 65,000 characters (a bit less than 64kB,
due to an internal header). The main reason for this change --- apart
from sheer convenience --- is the `qubes.VMShell` service. It's an
extremely dangerous service that gives complete shell access.
Unsurprisingly, it's not allowed by default. In Qubes 4.0,
`qubes.VMShell` just provides a shell stdin/stdout, so handling it with
a single command was extremely cumbersome and often required a
prodigious number of quotation marks. Even worse, the command was not
stored in any log file, and qrexec policy did not control what commands
could be executed, opening up a big, unauditable, dangerous space for
exploitation. By increasing the service+argument length (and governing
the argument under qrexec policy, as described above), we are able to
introduce a new, more accountable, safer service. In Qubes 4.1, this new
service is called `qubes.VMExec`, and the complete command to be
executed, along with its arguments, is logged. This allows for precise
control, as the policy can allow only certain commands, not everything
that can be executed in a shell. Moreover, all the arguments are
explicitly defined and unambiguous, freeing the user from quotation mark
purgatory. (The syntax is described
[here](/doc/vm-interface/#qubes-rpc).) Other
services will also benefit, such as the U2F implementation that, due to
previous limitations, required some awkward wrangling of the public key
hash. (Details can be found
[here](https://github.com/QubesOS/qubes-issues/issues/4850).)

There are also some minor improvements and streamlining coming up: for
example, a new call that allows access to all qube properties at once,
which allows `qvm-ls` (and similar tools) to work significantly faster.
Instead of making around five Admin API calls per qube, `qvm-ls` will
now only make one, leading to a five-fold speed increase.

### Policy daemon

In Qubes 4.0, performing a qrexec call (with all its assorted policy
checking) was a fairly slow process. Even in the best case (simple
allow, no asking user for input), it consisted of several processes:
loading the whole policy again, loading Python modules required to parse
it, executing the call itself.... The setup alone took about 300 ms (for
a typical system). In the case of a lone operation, such as copying a
file, it's hardly a problem, but for frequent operations it could be a
disaster. Tunneling a TCP connection via qrexec (the `qubes.ConnectTCP`
service) would mean a 300 ms lag for establishing the connection --- a
complete nightmare for the server! Qubes Clipboard, which also uses
qrexec and is managed by a policy (which allows for blocking copy or
paste for certain qubes), needed 300 ms for each operation, which can be
a bit of a problem for particularly fast typists. The biggest problem
was, however, the Admin API. The whole idea of an Admin API (read more
[here](/news/2017/06/27/qubes-admin-api/))
requires a lot of qrexec calls. If we want to implement a GUI domain
that manages certain parts of the system instead of having the user
directly access dom0, we need the Admin API to be much, much faster. For
example, without the changes outlined below, starting the Qube Manager
on a default, out-of-the-box Qubes installation with a GUI domain took
37 seconds. This is just unacceptable.

To speed up qrexec calls, Qubes 4.1 introduces a policy daemon. It is a
process that loads the policy into memory and keeps it there (only
reloading when the policy files change), answering qrexec queries about
whether a given operation should be accepted or denied without spawning
new processes or loading more files and Python modules. The results are
quite satisfying. Starting the Qube Manager in a default Qubes
installation with a GUI domain takes less than 5 seconds --- a more than
seven-fold improvement.

### Socket services

Another problematic bottleneck for qrexec was that each service was a
new process. In many cases, this process just connects to an existing
service. A particularly egregious example is the Admin API. An Admin API
call is just a connection to dom0's qubesd daemon, but in Qubes 4.0 each
of its calls was a separate process, going through a bunch of shell
scripts to return a simple chunk of data. For a program like `qvm-ls`,
which has to access a lot of data from the Admin API, it was a
performance nightmare.

A qrexec service is a file residing in `/etc/qubes-rpc/`. In Qubes 4.0,
it has to be an executable or a symlink to one. In Qubes 4.1, however,
it can also be a symlink to a unix socket. Via this socket, you can
simply connect to an existing process that will handle your call. With
each socket connection, this running service receives a new connection
with a metadata header (containing information on who wants to do what
and where, a service name just in case, and a terminating 0 byte)
followed by the same client data as before. Thus, even though it's a
single process that handles multiple calls, it knows where each call
came from and what it asked for.

Performance gains on both speed and memory benchmarks are significant.
For example, we have an icon rendering service, which is responsible for
making sure each of your program icons bears the color of its qube. In
Qubes 4.0, each invocation of this service was a separate per-qube
processes in dom0, each one loading Gtk and other libraries for about
32MB of RAM. In Qubes 4.1, this has been converted into a
single-process, socket-based service. Now, ten running qubes need only
this one socket-based service process, for a single instance of 32MB RAM
usage, instead of ten times that in Qubes 4.0. The Admin API also uses a
socket service, which --- combined with the policy daemon described
above --- dropped call execution time from 300 ms to about 50 ms. 

More information about socket services can be found
[here](https://github.com/QubesOS/qubes-issues/issues/3912).

## Notifications

Creating a qrexec policy daemon opened another convenient opportunity:
adding notifications for some policy actions. By default, notifications
are shown only when a *deny* occurs, unless it was due to an *ask*
action. (If the user was already asked for consent, it is not necessary
to show the notification.) However, the new policy format allows for an
optional `notify=no` parameter with *deny* rules. *Allow* rules support
a `notify=yes` parameter that will show a notification on accepted
qrexec calls.

[![Screenshot of an allow and a deny notifications](/attachment/posts/qrexec1.png)](/attachment/posts/qrexec1.png)
 
For example, this rule will cause notifications to be shown whenever a
`qubes.VMExec` service call is made from the `work` qube to the
`personal` qube:

```
qubes.VMExec * work personal allow notify=yes
```

And the above screenshot could be accomplished with:

```
qubes.Gpg * work work-gpg allow notify=yes
qubes.Filecopy * untrusted * deny
```

Notifications alert the user that something unexpected might have
happened. They can signal a misconfigured policy (when calls are denied
while the user would prefer them to be allowed) or a misbehaving or
malicious qube (when unexpected calls are being made and denied due to a
correctly configured policy).

The introduction of GUI domains makes the subject of qrexec
notifications a bit more complex. Since there can be more than one GUI
domain, which one should receive notifications? For now, we've decided
that the GUI domain serving the source qube (the one making the call)
will show notifications. There are some conceivable setups in which a
different GUI domain would show notifications. While such setups will
not be supported in Qubes 4.1, they may appear in future versions.

More information about notifications can be found
[here](https://github.com/QubesOS/qubes-issues/issues/3904).

## Policy API (in development)

As we mentioned above, one of our big development goals is separating
the user from the most vulnerable part of the system: dom0. Doing so
improves compartmentalization and security while lowering both the
attack surface and the chance of accidentally committing a compromising
mistake.

In order to completely isolate dom0, however, we would have to provide
qrexec interfaces for everything that the user may need to access and
configure --- including the qrexec policy itself. At the moment, we are
working on a broad Policy API that allows for editing the whole policy.
The Policy API will also enforce the correctness of any policy changes,
preventing users from making syntactically invalid policy changes.

We also have some ideas for implementing limited access, for example,
having a deprivileged user who is limited to modifying certain services
or controlling access to certain qubes. The design for this is not yet
final. In its current state, we are thinking about supporting two kinds
of roles: an API administrator with total access, and an operator with
access to only a well-defined subset of the policy. The implementation
of any such ideas will definitely not be a part of Qubes 4.1.

Current work on the Policy API design draft and a proof of concept can
be found [here](https://github.com/QubesOS/qubes-policy-control).

