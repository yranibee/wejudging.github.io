---
title: Azure Linux 磁盘扩容
date: 2021-05-10
tags: [Azure,Microsoft]
categories: 折腾
cover: https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/azure.png
---

进入 azure 改磁盘大小,重启后进入系统会发现磁盘大小没变，因为没扩容。

本系统为centos7

#### 安装 growpart
```bash
yum install -y epel-release
```

#### 安装 growpart
```bash
yum install -y cloud-utils
```

#### 修改系统语言为为英文语言与编码
```bash
LANG=en_US.UTF-8
```

#### 扩-容块设备并重启
```bash
growpart /dev/sda 2
reboot
```

#### 重启后执行
##### xfs 文件系统 (azure)
```bash
xfs_growfs /dev/sda2
```
##### ext4 文件系统
```bash
resize2fs /dev/sda2 
```

#### 查看是否 ok
```bash
df -TH
```



