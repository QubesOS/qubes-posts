---
layout: post
title: TrenchBoot Anti Evil Maid for Qubes OS
author: Michal Zygowski
categories: articles
image: /attachment/posts/trenchboot-logo.png
---

_**Editor's note:** The following is a guest post by Michal Zygowski from [3mdeb](https://3mdeb.com/) on the work they've been doing to upgrade [Anti Evil Maid (AEM)](/doc/anti-evil-maid/). The original post can be found on the [3mdeb blog](https://blog.3mdeb.com/2023/2023-01-31-trenchboot-aem-for-qubesos/). This work was made possible through generous [donations](/donate/) from the Qubes community via [OpenCollective](https://opencollective.com/qubes-os). We are immensely grateful to the Qubes community for your continued support and to 3mdeb for contributing this valuable work._

## Abstract

Qubes OS Anti Evil Maid (AEM) software heavily depends on the
availability of the DRTM technologies to prevent the Evil Maid
attacks. However, the project has not evolved much since the
beginning of 2018 and froze on the support of TPM 1.2 with Intel TXT
in legacy boot mode (BIOS). In the post we show how existing
solution can be replaced with TrenchBoot and how one can install it
on the Qubes OS. Also the post will also briefly explain how
TrenchBoot opens the door for future TPM 2.0 and UEFI support for
AEM.

## Introduction

As Qubes OS users, promoters, and developers, we understand how essential it is
to be aware of the latest developments in maintaining the security of your
favorite operating system. We're excited to share our plans to integrate the
TrenchBoot Project into Qubes OS's new Anti-Evil Maid (AEM) implementation. As
you may know, traditional firmware security measures like UEFI Secure Boot and
measured boot, even with a Static Root of Trust (SRT), may only sometimes be
enough to ensure a completely secure environment for your operating system.
Compromised firmware may allow for the injection of malicious software into
your system, making it difficult to detect. To overcome these limitations, many
silicon vendors have started implementing Dynamic Root of Trust (DRT)
technologies to establish a secure environment for operating system launch and
integrity measurements. We're excited to take advantage of these advancements
through integration with the [TrenchBoot Project](https://trenchboot.org/).

The usage of DRT technologies like Intel Trusted Execution Technology (TXT) or
AMD Secure Startup is becoming more and more significant; for example, Dynamic
Root of Trust for Measurement (DRTM) requirements of [Microsoft Secured Core PCs](https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-highly-secure#what-makes-a-secured-core-pc).
DRTM has yet to find its place in open-source projects, but that gradually
changes. The demand for having firmware-independent Roots of Trust is
increasing, and projects that satisfy this demand are growing TrenchBoot is a
framework that allows individuals and projects to build security engines to
perform launch integrity actions for their systems. The framework builds upon
Boot Integrity Technologies (BITs) that establish one or more Roots of Trust
(RoT) from which a degree of confidence that integrity actions were not
subverted.

[Qubes OS Anti Evil Maid (AEM)](https://blog.invisiblethings.org/2011/09/07/anti-evil-maid.html)
software heavily depends on the availability of DRTM technologies to prevent
Evil Maid attacks. However, the project hasn't evolved much since the beginning
of 2018 and froze on the support of TPM 1.2 with Intel TXT in legacy boot mode
(BIOS). Because of that, the usage of this security software is effectively
limited to older Intel machines only. TPM 1.2 implemented SHA1 hashing
algorithm, which is nowadays considered weak in the era of forever-increasing
computer performance and quantum computing. The solution to this problem comes
with a newer TPM 2.0 with more agile cryptographic algorithms and SHA256
implementation by default.

The post will present the TrenchBoot solution for Qubes OS AEM replacing the
current TPM 1.2 and Intel TXT-only implementation. The advantage of TrenchBoot
solution over existing [Trusted Boot](https://sourceforge.net/p/tboot/wiki/Home/)
is the easier future integration of AMD platform support, as well as TPM 2.0
and UEFI mode support.

Before we dive into the technical details, it is important to highlight that
this achievement was made possible through the generous contributions of Qubes
OS community via OpenCollective. We would like to express our gratitude and
extend a special thank you to all who have supported our favourite operating
system. To continue supporting Qubes OS, please consider donating through
[OpenCollective page](https://opencollective.com/qubes-os). Thank you for your
continued support!

## Modificationts to original Qubes OS AEM

To replace the original implementation of Qubes OS AE
there weren't any AEM scripts modifications necessary. What actually had to
change is GRUB and Xen Hypervisor (and Trusted Boot - to be removed). Why? one
may ask... First of all, one must understand the role of a Trusted Boot
(TBOOT).

## Trusted Boot DRTM flow

[![Breakout of measured launch details](/attachment/posts/txt_launch2.jpg)](/attachment/posts/txt_launch2.jpg)

(Source: [A Practical Guide to TPM 2.0](https://link.springer.com/book/10.1007/978-1-4302-6584-9))

The main role of Trusted Boot was to prepare a platform to be launched with
Intel TXT (Intel's DRTM technology) in an operating system agnostic way. It has
been achieved by loading a tboot kernel with Multiboot protocol and the other
system components as the modules. That way, TBOOT is the main kernel that
starts first and prepares the platform for TXT launch. When the platform is
ready, then tboot performs the TXT launch. The control is passed to SINIT
Authenticated Code Module (ACM), a binary signed and provided by Intel designed
for DRTM technology. SINIT ACM uses TXT to measure the operating system
components in a secure manner. Then the control is handed back to the tboot
kernel, which checks if the operation was successful and boots the target
operating system.

Although the tboot tried to be as OS agnostic as possible, some tboot presence
awareness from the operating system is needed because the application processor
cores (all cores except the main one) are left in a special state after TXT
launch and cannot be woken up like in traditional boot process. To solve this
problem, tboot installs a special processor wakeup procedure in the memory,
which OS must call into to start the processor cores. Only then OS may
initialize the processor per its own requirements.

As one can see, the process is complex in the case of Intel TXT. Migration of
all tboot responsibilities was not trivial and has been divided into the work
on both GRUB and Xen Hypervisor side of Qubes OS.

## GRUB modifications

[![GRUB logo](/attachment/posts/grub_logo.png)](/attachment/posts/grub_logo.png)

In order to fulfill the same role as tboot, GRUB had to learn how to prepare
the platform and perform the TXT launch. Most of the work for that particular
part has been done by [Oracle Team working on TrenchBoot for GRUB](https://www.mail-archive.com/grub-devel@gnu.org/msg30167.html).
That work, however, covered the Linux kernel TXT launch only. What still had to
be done was the Multiboot2 protocol support in GRUB to be able to TXT launch a
Xen Hypervisor. The patches have been prepared for the respective Qubes GRUB
[package](https://github.com/3mdeb/qubes-grub2/pull/2).

## Xen modifications

[![Xen Project logo](/attachment/posts/xen_project_logo.png)](/attachment/posts/xen_project_logo.png)

Analogically to GRUB, Xen had to take over some responsibilities from tboot.
Due to the Intel TXT requirements for the boot process, a new entry point had
to be developed to which SINIT ACM will return control. The new entry point was
responsible for saving information that a TXT launch happened and cleaning up
the processor state so that the booting of the Xen kernel could continue with
the standard Multiboot2 path. Among others, if Xen detected TXT launch, it had
to perform the special processor cores wakeup process (which has been rewritten
from TrenchBoot Linux patches to Xen native code) and measure external
components before using them (that is the Xen parameters, Dom0 Linux kernel,
initrd and Dom0 parameters). Xen also had to reserve the memory regions used by
Intel TXT, as when tboot was used. The relevant source code for the respective
Qubes Xen package is available [here](https://github.com/3mdeb/qubes-vmm-xen/pull/1).

## Installation and verification of TrenchBoot AEM on Qubes OS

For a seamless deployment and installation of TrenchBoot AEM, the modifications
Qubes OS components compilation. Those patches have been presented earlier with
have been converted to patches which are applied to projects' sources during
links to Pull Requests. It allows building ready-to-use RPM packages that can
be installed directly on an installed Qubes OS system. The pre-built packages
can be downloaded from [here](https://3mdeb.com/open-source-firmware/QubesOS/trenchboot_aem_poc/).
The packages have been covered with SHA512 sums signed with 3mdeb's
`Qubes OS TrenchBoot AEM open-source software release 0.x signing key`
available on [3mdeb-secpack repository](https://github.com/3mdeb/3mdeb-secpack/blob/master/open-source-software/qubes-os-trenchboot-aem-open-source-software-release-0.x-signing-key.asc).
To verify the RPM packages, fetch the key with the following command:
`gpg --fetch https://raw.githubusercontent.com/3mdeb/3mdeb-secpack/master/open-source-software/qubes-os-trenchboot-aem-open-source-software-release-0.x-signing-key.asc`
and then to verify the packages, please run:

```bash
$ gpg --verify sha512sums.sig sha512sums

gpg: Signature made wto, 31 sty 2023, 11:06:06 CET
gpg:                using RSA key 3405D1E4509CD18A3EA762245D289020C07114F3
gpg: Good signature from "Qubes OS TrenchBoot AEM open-source software release 0.x signing key" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 3405 D1E4 509C D18A 3EA7  6224 5D28 9020 C071 14F3

$ sha512sum -c sha512sums

grub2-common-2.06-1.fc32.noarch.rpm: OK
grub2-tools-extra-2.06-1.fc32.x86_64.rpm: OK
xen-licenses-4.17.0-3.fc32.x86_64.rpm: OK
grub2-pc-2.06-1.fc32.x86_64.rpm: OK
xen-libs-4.17.0-3.fc32.x86_64.rpm: OK
grub2-tools-2.06-1.fc32.x86_64.rpm: OK
xen-hypervisor-4.17.0-3.fc32.x86_64.rpm: OK
grub2-pc-modules-2.06-1.fc32.noarch.rpm: OK
xen-runtime-4.17.0-3.fc32.x86_64.rpm: OK
grub2-tools-minimal-2.06-1.fc32.x86_64.rpm: OK
python3-xen-4.17.0-3.fc32.x86_64.rpm: OK
xen-4.17.0-3.fc32.x86_64.rpm: OK
```

Check if GPG returns a good signature and if yes, check if the RPM checksum
matches. All files must be in the same directory for the procedure to work.

Note, in order to use the TrenchBoot AEM for Qubes OS, you have to own a
TXT-capable platform with TXT-enabled firmware offering legacy boot. Such
platform can be Dell OptiPlex 7010. You can visit
[Dasharo with Intel TXT support blog post](https://blog.3mdeb.com/2022/2022-03-17-optiplex-txt/)
to learn more about such hardware and firmware. If you want to get OptiPlex
with Dasharo pre-installed, you can get one from
[3mdeb shop](https://3mdeb.com/shop/open-source-hardware/dasharo-dell-optiplex-7010-sff-i3-i7-8gb-32gb-ram-copy/).

## Building Xen and GRUB packages

If you are not interested in compilation, skip to the [next section](#installing-xen-and-grub-packages).

To not make the post excessively long, the procedure for building packages
has been put into [TrenchBoot SDK documentation](https://github.com/TrenchBoot/trenchboot-sdk/blob/3d56ca7b27bb038629fd838819a1050006725a1e/Documentation/build_qubes_packages.md).
Follow the instructions in the file to build the TrenchBoot AEM packages.

## Installing Xen and GRUB packages

The following process was carried out and tested on
[Qubes OS 4.2](https://openqa.qubes-os.org/tests/55506#downloads).

In order to install the packages one has to send the Xen and GRUB RPMs to the
Dom0. Please note that moving any external files or data to Dom0 is potentially
dangerous. Ensure that your environment is safe and the RPMs have the right
checksums after copying them to Dom0. If you don't know how to copy files to
Dom0, refer to the [Qubes OS documentation](https://doc.qubes-os.org/en/latest/user/how-to-guides/how-to-copy-from-dom0.html#copying-to-dom0).

1. Even before installing packages, it is required to enable the
   `current-testing` repository to avoid the need to install additional
   dependencies:

    ```bash
    sudo qubes-dom0-update --enablerepo=qubes-dom0-current-testing
    ```

2. If the RPMs are inside Dom0, install them with the following command
   (assuming you downloaded all of them to one directory):

    ```bash
    sudo dnf update \
        python3-xen-4.17.0-3.fc32.x86_64.rpm \
        xen-4.17.0-3.fc32.x86_64.rpm \
        xen-hypervisor-4.17.0-3.fc32.x86_64.rpm \
        xen-libs-4.17.0-3.fc32.x86_64.rpm \
        xen-licenses-4.17.0-3.fc32.x86_64.rpm \
        xen-runtime-4.17.0-3.fc32.x86_64.rpm \
        grub2-common-2.06-1.fc32.noarch.rpm \
        grub2-pc-modules-2.06-1.fc32.noarch.rpm \
        grub2-pc-2.06-1.fc32.x86_64.rpm \
        grub2-tools-2.06-1.fc32.x86_64.rpm \
        grub2-tools-extra-2.06-1.fc32.x86_64.rpm \
        grub2-tools-minimal-2.06-1.fc32.x86_64.rpm
    ```

3. Invoke `sudo grub2-install /dev/sdX`, where X is the letter representing the
   disk with `/boot` partition.
4. Additionally, you will have to download SINIT ACM and place it in `/boot`
   partition/directory so that GRUB will be able to pick it up. Note it is only
   necessary if your firmware/BIOS does not include/place SINIT ACM in the
   Intel TXT region. You may obtain all SINIT ACMs as described
   [here](https://github.com/QubesOS/qubes-antievilmaid/blob/7561a4d724b9b0df8ba48d8f2735d3754961f87b/README#L177).
   Copy the SINIT ACM suitable for your platform to `/boot` directory. In the
   case of Dell OptiPlex it will be `SNB_IVB_SINIT_20190708_PW.bin`.
5. Install Qubes AEM packages with the following command because Qubes OS 4.2
   lacks AEM packages:

    ```bash
    qubes-dom0-update  --enablerepo=qubes-dom0-current-testing anti-evil-maid
    ```

6. Enter the SeaBIOS TPM menu (hotkey `t`) and choose the clear TPM option.
   Then activate and enable the TPM by selecting the appropriate options. If in
   any case you are using proprietary firmware, clear the TPM and then enable
   and activate it in the firmware setup application.
7. Follow the steps in [set up TPM for AEM](https://github.com/QubesOS/qubes-antievilmaid/blob/7561a4d724b9b0df8ba48d8f2735d3754961f87b/README#L147).
8. The anti-evil-maid script may not work with LUKS2 in its current state, so
   make a fix according to this [Pull Request](https://github.com/QubesOS/qubes-antievilmaid/pull/41/files)
   if needed.
9. Now it is possible to [setup Qubes OS AEM device](https://github.com/QubesOS/qubes-antievilmaid/blob/7561a4d724b9b0df8ba48d8f2735d3754961f87b/README#L202).
   This will create the AEM entry in Qubes GRUB, but this entry is using tboot.
10. You will need to edit the grub configuration file (`/boot/grub2/grub.cfg`)
    by copying the standard Qubes OS entry (without AEM) and adding:

    ```bash
    slaunch
    slaunch_module /<name_of_the_sinit_acm>
    ```

    before the `multiboot2` directive, which loads Xen Hypervisor. Name the
    entry differently, e.g. `Qubes OS with TrenchBoot AEM`. Also, you will need
    to copy the AEM parameters for the Linux kernel: e.g.:

    ```txt
    aem.uuid=38474da6-7b2d-410d-95e6-8683005fb23f rd.luks.key=/tmp/aem-keyfile rd.luks.crypttab=no
    ```
    We are still working on automating this step, so please bear with the
    manual file edition for now.

Example GRUB entry:

```bash
menuentry 'Qubes, with TrenchBoot AEM' --class qubes --class gnu-linux --class gnu --class os --class xen $menuentry_id_option 'xen-gnulinux-simple-/dev/mapper/qubes_dom0-root' {
        insmod part_msdos
        insmod ext2
        set root='hd0,msdos1'
        if [ x$feature_platform_search_hint = xy ]; then
          search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 --hint='hd0,msdos1'  38474da6-7b2d-410d-95e6-8683005fb23f
        else
          search --no-floppy --fs-uuid --set=root 38474da6-7b2d-410d-95e6-8683005fb23f
        fi
        echo    'Loading Xen 4.17.0 ...'
        if [ "$grub_platform" = "pc" -o "$grub_platform" = "" ]; then
            xen_rm_opts=
        else
            xen_rm_opts="no-real-mode edd=off"
        fi
        slaunch
        slaunch_module  /SNB_IVB_SINIT_20190708_PW.bin
        multiboot2      /xen-4.17.0.gz placeholder  console=none dom0_mem=min:1024M dom0_mem=max:4096M ucode=scan smt=off gnttab_max_frames=2048 gnttab_max_maptrack_frames=4096 ${xen_rm_opts}
        echo    'Loading Linux 5.15.81-1.fc32.qubes.x86_64 ...'
        module  /vmlinuz-5.15.81-1.fc32.qubes.x86_64 placeholder root=/dev/mapper/qubes_dom0-root ro rd.luks.uuid=luks-f1f850fa-59bf-4911-8256-4986c485e112 rd.lvm.lv=qubes_dom0/root rd.lvm.lv=qubes_dom0/swap i915.alpha_support=1 rd.driver.pre=btrfs rhgb quiet console=ttyS0,115200  aem.uuid=38474da6-7b2d-410d-95e6-8683005fb23f rd.luks.key=/tmp/aem-keyfile rd.luks.crypttab=no
        echo    'Loading initial ramdisk ...'
        module2 --nounzip   /initramfs-5.15.81-1.fc32.qubes.x86_64.img
}
```

You may put the above entry to `/etc/grub.d/40_custom` to avoid the removal of
the TrenchBoot AEM entry in case `grub2-mkconfig` will be called to overwrite
`grub.cfg`. Please note such a workaround will not allow for automatic Xen,
Linux, and initrd updates which may include security fixes. Making the
`grub.cfg` seamless for TrenchBoot is still in progress.

## Verifying TrenchBoot AEM for Qubes OS

The moment of truth has come. If the installation has been performed
successfully, it is time to try out the TXT launch. So reboot the platform and
choose the newly created entry with TrenchBoot. If it succeeds, you should get
a TPM SRK and LUKS password prompts.

After the system boots, one may check if DRTM PCRs (17 and 18, 19 is not used
by TrenchBoot for now) have been populated:

```
cat /sys/class/tpm/tpm0/pcrs
PCR-00: 3A 3F 78 0F 11 A4 B4 99 69 FC AA 80 CD 6E 39 57 C3 3B 22 75
PCR-01: 4D E4 B0 42 71 50 E4 B1 DE C0 D7 F1 A0 29 A2 65 11 30 72 FD
PCR-02: CE EA EC 0A 01 D5 7B A3 55 5A 4C 02 59 4A EE A1 C9 41 78 FB
PCR-03: 3A 3F 78 0F 11 A4 B4 99 69 FC AA 80 CD 6E 39 57 C3 3B 22 75
PCR-04: 01 7A 3D E8 2F 4A 1B 77 FC 33 A9 03 FE F6 AD 27 EE 92 BE 04
PCR-05: BF 4E 38 B0 A7 7A 7A 4D 1A A9 B5 0F 59 D8 E5 F7 A6 46 8E 48
PCR-06: 3A 3F 78 0F 11 A4 B4 99 69 FC AA 80 CD 6E 39 57 C3 3B 22 75
PCR-07: 3A 3F 78 0F 11 A4 B4 99 69 FC AA 80 CD 6E 39 57 C3 3B 22 75
PCR-08: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-09: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-10: 9A 51 66 4D EB 1C B9 72 91 87 59 C4 89 AC 9A FF 7F 10 BF B3
PCR-11: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-12: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-13: 10 78 D0 16 8C 85 85 3A 7E 0A A1 D7 56 02 A7 05 D4 7F 22 64
PCR-14: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-15: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-16: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-17: 2A C9 64 F2 E2 96 50 B3 1D B7 2F 77 C4 7C A6 5D AA C8 4E E7
PCR-18: 84 4D D5 8D 95 EB 96 F6 CE 92 51 9C FD E2 33 45 71 C3 87 92
PCR-19: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-20: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-21: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-22: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
PCR-23: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```

Note that AEM will fail to unseal secrets as the PCRs are changed. To re-seal
the secret you will have to perform the following steps after a successful boot
with TrenchBoot:

1. Re-seal the secret `sudo anti-evil-maid-seal ""`.
2. Reboot the machine and notice that the anti-evil-maid service no longer
   fails during boot. The secret should be displayed on the screen, indicating
   the machine boots correctly and unseals the secret.

## Summary

It has been shown that TrenchBoot can be integrated to perform DRTM secure
launch of Qubes OS in place of old tboot. Moreover, TrenchBoot is more
extensible to other platforms like AMD. In the future, Anti Evil Maid will be
available on both Intel and AMD platforms with both TPM 1.2 and TPM 2.0, thanks
to TrenchBoot (which seemed to not be possible with tboot only).

If you think we can help in improving the security of your firmware or you are
looking for someone who can boost your product by leveraging advanced features
of used hardware platform, feel free to [book a call with us](https://calendly.com/3mdeb/consulting-remote-meeting)
or drop us email to `contact<at>3mdeb<dot>com`. If you are interested in
similar content, feel free to [sign up to our newsletter](https://newsletter.3mdeb.com/subscription/PW6XnCeK6)
