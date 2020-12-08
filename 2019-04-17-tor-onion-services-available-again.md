---
layout: post
title: "Qubes Tor onion services are available again!"
categories: announcements
author: Andrew David Wong
---

We previously announced that the [Qubes Tor onion services were no
longer being maintained][orig-onion-ann] due to lack of resources.
However, [Unman] generously agreed to bring them back, and they're now
available once again!

Here are the new onion service URLs:

```
Website:   www.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion
Yum repo:  yum.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion
Deb repo:  deb.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion
ISOs:      iso.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion
```

Soon, you will be able to get the new, correct repo definitions just by
updating dom0 and your TemplateVMs. However, if you can't wait, you can
edit your repository definitions by following the instructions below.

## Instructions

Follow these instructions **only if** you wish to update dom0 and your
TemplateVMs over Tor (via `sys-whonix`). This is an opt-in feature. If,
instead, you wish to update over your regular network connection (aka
"clearnet"), **or if you are not sure**, then **do not** follow these
instructions.

In order to use the new onion services, you must ensure that **every**
line that contains an onion address uses the appropriate **new** address
above. We'll go through this for dom0, Fedora templates, Debian, and
Whonix templates. For additional information, see [Onionizing
Repositories] on the Whonix wiki.

### dom0

1. In dom0, open `/etc/yum.repos.d/qubes-dom0.repo` in a text editor.

2. Comment out all the `baseurl = https://yum.qubes-os.org/[...]` and
   `metalink` lines.

3. Uncomment all the `baseurl = [...].onion` lines.

4. Update every `.onion` address to
   `yum.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion`.
   The affected lines should look like this:
   ```
   #baseurl = https://yum.qubes-os.org/r$releasever/current/dom0/fc25
   baseurl = http://yum.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion/r$releasever/current/dom0/fc25
   #metalink = https://yum.qubes-os.org/r$releasever/current/dom0/fc25/repodata/repomd.xml.metalink
   ```

5. Open `/etc/yum.repos.d/qubes-templates.repo` in a text editor and
   repeat steps 2-4.

6. In **Qubes Global Settings**, set **Dom0 UpdateVM** to `sys-whonix`.

### Fedora TemplateVMs

1. In the TemplateVM, open `/etc/yum.repos.d/qubes-r4.repo` in a text
   editor.

2. Comment out every line that contains `yum.qubes-os.org`.

3. Uncomment every line that contains `.onion`.

4. Update every `.onion` address to
   `yum.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion`.
   The affected lines should look like this:
   ```
   #baseurl = https://yum.qubes-os.org/r4.0/current/vm/fc$releasever
   baseurl = http://yum.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion/r4.0/current/vm/fc$releasever
   ```

5. In dom0, ensure that the first non-comment line in
   `/etc/qubes-rpc/policy/qubes.UpdatesProxy` is:
   ```
   $type:TemplateVM    $default    allow,target=sys-whonix
   ```

### Debian & Whonix TemplateVMs

1. In the TemplateVM, open `/etc/apt/sources.list.d/qubes-r4.list` in a
   text editor.

2. Comment out every line that contains `deb.qubes-os.org`.

3. Uncomment every line that contains `.onion`.

4. Update every `.onion` address to
   `deb.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion`.
   The affected lines should look like this:
   ```
   # Main qubes updates repository
   #deb [arch=amd64] https://deb.qubes-os.org/r4.0/vm stretch main
   #deb-src https://deb.qubes-os.org/r4.0/vm stretch main

   ```
   ```
   # Qubes Tor updates repositories
   # Main qubes updates repository
   deb [arch=amd64] http://deb.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion/r4.0/vm stretch main
   #deb-src http://deb.qubesosfasa4zl44o4tws22di6kepyzfeqv3tg4e3ztknltfxqrymdad.onion/r4.0/vm stretch main
   ```

5. In dom0, ensure that the first non-comment line in
   `/etc/qubes-rpc/policy/qubes.UpdatesProxy` is:
   ```
   $type:TemplateVM    $default    allow,target=sys-whonix
   ```


[Unman]: /team/#unman
[orig-onion-ann]: /news/2018/01/23/qubes-whonix-next-gen-tor-onion-services/
[Onionizing Repositories]: https://www.whonix.org/wiki/Onionizing_Repositories

