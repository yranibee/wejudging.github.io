---
title: Azure Linux 磁盘扩容
date: 2021-05-10
tags:
categories: linux
---

进入 azure 改磁盘大小,重启后进入系统会发现磁盘大小没变，因为没扩容。

本系统为 centos

```shell
#安装 growpart
yum install -y epel-release
```

```shell
#安装 growpart
 yum install -y cloud-utils
 ```
 