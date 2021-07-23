---
title: Microsoft 365 E5 开发者
date: 2021-04-26 14:12:39
tags: [onedrive,microsoft]
categories: 折腾
cover: https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/文章图片/Microsoft365.png
---



### 立即加入 Microsoft 365 开发人员计划
- 获取免费、可续订的 90 天 Microsoft 365 E5 开发人员订阅;
- 包括SharePoint、OneDrive、Outlook、Exchange、Teams、Planner、Word、Excel、PowerPoint；
- 订阅包含 25 个用于所有 Microsoft 365 应用的许可证
- OneDrive每个用户的网盘容量上限为 5TB；

申请链接：https://developer.microsoft.com/zh-cn/office/dev-program
Microsoft 365 E5 开发者管理页： https://admin.microsoft.com/

### 网盘扩容
打开 https://admin.onedrive.com 登录之后在左方菜单中选择「存储」该项，将默认存储改为 5120，即 5TB。
但是管理员账号的容量此刻依旧为1TB，接下来更改管理员容量。

### 修改全局管理员自身的5T容量
- 我们先安装最新的 SharePoint Online Management Shell → [下载地址](https://www.microsoft.com/zh-cn/download/details.aspx?id=35588)；
- 点击电脑左下角开始按钮，搜索PowerShell，然后单击Windows PowerShell，此时会弹出一个命令框。
依次输入命令：

```
#adminUPN为管理员邮箱，orgName为你设置的组织名
$adminUPN="admin@weijiajin.onmicrosoft.com"
$orgName="weijiajin"
#该步会弹出一个窗口，会要求你输入邮箱密码
$userCredential = Get-Credential -UserName $adminUPN -Message "Type the password."
Connect-SPOService -Url https://$orgName-admin.sharepoint.com -Credential $userCredential
#这里默认修改为5T，如果你要修改为其它的可自行修改，单位为M，最大可修改为5T
Set-SPOTenant -OneDriveStorageQuota 5242880
#将后面的地址修改成你的OneDrive网盘地址，地址仿照下面的即可
Set-SPOSite -Identity https://weijiajin-my.sharepoint.com/personal/admin_weijiajin_onmicrosoft_com -StorageQuota 5242880
```
**如果你要修改现有用户的容量的话，将最后一步的OneDrive网盘地址替换成你想修改的用户地址即可。**

### 微软OneDrive网盘免费升级到25T容量教程

如果OneDrive 5T不够用，这里分享个免费升级25T的方法，也是微软很早就出的一个政策，部分订阅的OneDrive网盘使用量超过90%的可免费申请提高容量到25T，有需求的可以升级下。

### 申请方法（未试）

提示：以下申请操作都需要全局管理员操作，如果需求大，也可以给自己其它的账号也升级到25T。

- 管理员登录后台→[传送门](https://admin.microsoft.com/Adminportal/Home)，点击左侧支持-新建服务请求；

- 然后在帮助框写下： 请帮助我吧 admin@weijiajin.onmicrosoft.com 账户的 onedrive 容量升级至25T；

- 然后填上管理员邮箱，附件上传几张该账号容量超过90%的截图，包括账户信息；

- 最后发送即可，截图不规范的，中途可能会有工作人员打电话要你重新发图片给他；

- 最后等一天，成功的邮件就会发给你，然后这时候你就照着邮件给的方法自行升级到25T。

### Microsoft 365 E5 开发者 开源自动订阅程序 

教程地址：https://qyi.io/archives/687.html
项目地址：https://github.com/luoye663/e5
自动订阅程序： https://e5.qyi.io/


### 总结

Microsoft 365 E5 开发者 主要可以白嫖Office与onedrive 5T大容量还可以存储一些大文件，下载也很方便，如果可以一直自动续费那是相当不错的，值得推荐使用。比国内的网盘好太多，速度也很不错！office也没有广告，一些想用正版Office也很推荐。

### 后续补充

{% timeline %}

<!-- node 2021 年 6 月 16 日 -->

续订成功

{% image https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io@main/source/images/文章图片/365/365renew.png width:500px %}


{% endtimeline %}







