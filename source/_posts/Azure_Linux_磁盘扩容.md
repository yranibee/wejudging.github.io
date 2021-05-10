---
title: Azure Linux 磁盘扩容
date: 2021-05-10
tags:
categories: linux
---

进入 azure 改磁盘大小,重启后进入系统会发现磁盘大小没变，因为没扩容。

本系统为centos7

#### 安装 growpart

yum install -y epel-release

#### 安装 growpart

yum install -y cloud-utils

#### 解决编码问题

LANG=en_US.UTF-8

#### 扩-容块设备并重启

growpart /dev/sda 2

reboot

#### 重启后执行

xfs_growfs /dev/sda2 (xfs 文件系统) azure 执行这个
resize2fs /dev/sda2 (ext4 文件系统)

#### 查看是否 ok

df -TH


