---
layout: post
title: "Preparing your build your first kernel module"
date: 2018-07-20
image: ''
description:
category: ''
tags:
twitter_text:
introduction:
published: false
---

/usr/src/linux was not available

Used Yast2 to install kernel-devel for the appropriate kernel version, which is 4.4.73-5-default in my case.

host:/lib/modules/4.4.73-5-default/kernel # uname -a
Linux host 4.4.73-5-default #1 SMP Tue Jul 4 15:33:39 UTC 2017 (b7ce4e4) x86_64 x86_64 x86_64 GNU/Linux

When I tried to compile the module, I hit the following error:

```
make -C /usr/src/linux SUBDIRS=/root/driver/hello-world modules
make[1]: Entering directory '/usr/src/linux-4.4.73-5'

  ERROR: Kernel configuration is invalid.
         include/generated/autoconf.h or include/config/auto.conf are missing.
         Run 'make oldconfig && make prepare' on kernel src to fix it.


  WARNING: Symbol version dump ./Module.symvers
           is missing; modules will have no dependencies and modversions.

  Building modules, stage 2.
scripts/Makefile.modpost:42: include/config/auto.conf: No such file or directory
make[2]: *** No rule to make target 'include/config/auto.conf'.  Stop.
Makefile:1431: recipe for target 'modules' failed
make[1]: *** [modules] Error 2
make[1]: Leaving directory '/usr/src/linux-4.4.73-5'
Makefile:13: recipe for target 'default' failed
make: *** [default] Error 2
```

The post here explained that this is expected in a newly downloaded kernel source which has never been compiled before.

I copied the /boot/config-4.4.73-5-default into /usr/src/linux folder, and ran make oldconfig

```
host:~/driver/hello-world # cd /usr/src/linux
host:/usr/src/linux # ls
Documentation  Kconfig   arch   certs   drivers   fs       init  kernel  mm   samples  security  tools  virt
Kbuild         Makefile  block  crypto  firmware  include  ipc   lib     net  scripts  sound     usr
host:/usr/src/linux # cp /boot/config-4.4.73-5-default .
host:/usr/src/linux # ls
Documentation  Kconfig   arch   certs                    crypto   firmware  include  ipc     lib  net      scripts   sound  usr
Kbuild         Makefile  block  config-4.4.73-5-default  drivers  fs        init     kernel  mm   samples  security  tools  virt
host:/usr/src/linux # make oldconfig
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  SHIPPED scripts/kconfig/zconf.tab.c
  SHIPPED scripts/kconfig/zconf.lex.c
  SHIPPED scripts/kconfig/zconf.hash.c
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
scripts/kconfig/conf  --oldconfig Kconfig
#
# using defaults found in /boot/config-4.4.73-5-default
#
#
# configuration written to .config
#
host:/usr/src/linux #
```

A new file called .config is created, interestingly the diff between this and the config-4.4.73-5-default file is below:

```
dl37168:/usr/src/linux # diff .config config-4.4.73-5-default
3c3
< # Linux/x86 4.4.73 Kernel Configuration
---
> # Linux/x86_64 4.4.72 Kernel Configuration
```

Make prepare did not work
```
dl37168:/usr/src/linux # make prepare
scripts/kconfig/conf  --silentoldconfig Kconfig
make[1]: *** No rule to make target 'arch/x86/entry/syscalls/syscall_32.tbl', needed by 'arch/x86/entry/syscalls/../../include/generated/asm/syscalls_32.h'.  Stop.
arch/x86/Makefile:202: recipe for target 'archheaders' failed
make: *** [archheaders] Error 2
```

