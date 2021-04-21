---
title: GitHub Actions 实现 Hexo 自动部署
cover: /images/actions.png
date: 2021-04-21
tags: [Hexo, GitHub]
categories: [笔记]
---

### Hexo
首先我们先要在本地确保 Hexo 是可以正确运行的，比如：

	hexo clean
	hexo deploy

确认 _config.yml 文件中有类似如下的 GitHub Pages 配置：


	deploy:
	  type: git
	  repository: https://github.com/wejudging/wejudging.github.io.git
	  branch: main

> 注意：请将 repository 修改为你自己的仓库地址。

### 生成秘钥
如果`Hexo`可以正常部署到`GitHub`，那么原来的秘钥是可以正常使用的;
但是私钥还要用于不同的服务器的`SSH` 访问和其他身份验证，因此生成一个新的秘钥对来专门部署`Hexo`。

以下为`macOS`下的操作，`Linux`下操作方法相同：
```shell
ssh-keygen -t rsa -b 4096 -C "Hexo Deploy Key" -f github-deploy-key -N ""
```
这会在当前目录生成两个文件：
- `github-deploy-key` —— 私钥
- `github-deploy-key.pub` —— 公钥

### `GitHub`配置秘钥

* 把私钥放到我们存放 Hexo 原始文件的代码仓库里面，用于触发 Actions 时使用;
* 把公钥放到 GitHub Pages 对应的代码仓库里面，用于 Hexo 部署时的写入工作。

#### 配置私钥
- 首先在`GitHub`上打开保存`Hexo`的仓库，访问 `Settings -> Secrets`，然后选择 `New secret`;
- 名字部分填写：`HEXO_DEPLOY_KEY`，注意大小写，这个后面的 `GitHub Actions Workflow` 要用到;
- 在 `Value`的部分填入 github-deploy-key 中的内容。

#### 添加公钥

- 接下来我们需要访问存放网页的仓库，也就是`Hexo`部署以后的仓库，访问 `Settings -> Deploy keys`;
- 按 `Add deploy key` 来添加一个新的公钥；
- 在 `Title`中输入：`HEXO_DEPLOY_PUB` 字样，当然也可以填写其它自定义的名字;
- 在 `Key` 中粘贴 `github-deploy-key.pub`文件的内容;

>> 注意：一定要勾选 Allow write access 来打开写权限，否则无法写入会导致部署失败。

### 创建 `Workflow`







