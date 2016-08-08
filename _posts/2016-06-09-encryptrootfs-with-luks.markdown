---
layout: post
title:  "Encrypting your root filesystem with LUKS in cloud"
date:   2016-06-09 00:00:00 +0200
categories: log-book
---
***

Reboot stage

[root@localhost ~]# dracut --print-cmdline
 rd.luks.uuid=luks-6fc6ceea-b82b-46ea-81a8-ba15e59684e0 root=/dev/mapper/rootfs rootflags=rw,relatime,data=ordered rootfstype=ext3

After reboot system goes to infinity wait loop. It waits for some device by id.

to pass correct root kernel parameter to avoid auto generated behaviour
In this hook the most important environment variable is defined: root. The second one is rootok, which indicates,
that a module claimed to be able to parse the root defined. So for example, root=iscsi:.... will be claimed by
the iscsi dracut module, which then sets rootok.

***

Text


[etc_shadow-password-hash-formats]:http://www.aychedee.com/2012/03/14/etc_shadow-password-hash-formats/
[shadow-hash-generation]:http://unix.stackexchange.com/questions/81240/manually-generate-password-for-etc-shadow
[sed]:http://www.grymoire.com/Unix/Sed.html#uh-4

