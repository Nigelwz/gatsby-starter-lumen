---
title: Custom ubuntu kernel image
date: "2022-07-24T21:28:37.121Z"
template: "post"
draft: false
slug: "kernel"
category: "kernel"
tags:
  - "kernel"
  - "ubuntu"
description: ""
socialImage: "/media/river.jpg"
---

# Build custom linux in ubuntu system.

- Scenario: I want to build a linux image it supports "KASAN" debug function.
- You need to search linux distribution version.
```clike
console: uname -rm
```
- Download the linux distribiton and check the "Tag" is same with your linux version.
- Copy linux config file in the source code folder.
```clike
sudo cp /boot/$(uname -rm) .config
```
- Don't select below CONFIG
```clike=
CONFIG_MODULE_SIG_ALL=n
CONFIG_MODULE_SIG_KEY=n
CONFIG_SYSTEM_TRUSTED_KEYS=n
CONFIG_DEBUG_INFO=n
```
- Select below config
```clike=
CONFIG_KASAN=y
```

- Build image.
```clike=
sudo make-kpkg --initrd kernel-headers kernel_image
```
- If the build is ok, it can gen "deb" file in previews path.
```clike=
eric@ubuntu:cd ..
eric@ubuntu:~/kernel$ ls -ll *deb
-rw-r--r-- 1 root root 10926888 Jul 23 08:47 linux-headers-5.13.0_5.13.0-10.00.Custom_amd64.deb
-rw-r--r-- 1 root root 95304040 Jul 23 08:55 linux-image-5.13.0_5.13.0-10.00.Custom_amd64.deb
```

- Modify grub script.
```clike=
in the /etc/default/grub
comment "GRUB_TIMEOUT_STYLE=hidden"
set "GRUB_TIMEOUT = 10"
eric@ubuntu: update-grub
eric@ubuntu: reboot
```
- Finally, user try to  reboot the system, the boot ment will show the new linux distribution.





