---
layout: wiki
wiki: shell
order: -202102
seo_title: 实用脚本集合
title: shell
cover: true
logo:
  src: https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/shell.png
  small: 120px
  large: 240px
description: 实用脚本集合
---

#### tcp脚本

```bash
wget "https://raw.githubusercontent.com/wejudging/shell/main/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

#### speedtest测速脚本

```bash
wget -O speedtest-cli https://raw.githubusercontent.com/wejudging/shell/main/speedtest.py
chmod +x speedtest-cli
python speedtest-cli
```

#### 生成秘钥

```bash
ssh-keygen -t rsa -b 4096 -C "Deploy Key" -f github-deploy-key -N ""
```

当前目录生成两个文件：
- github-deploy-key —— 私钥
- github-deploy-key.pub —— 公钥

#### 图标生成

```bash
https://img.shields.io/badge/XXX-YYY-f38020?logo=cloudflare&logoColor=f38020&labelColor=282d33
```

#### mac一键开启hdpi
```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/xzhih/one-key-hidpi/master/hidpi.sh)"
```
#### x-ui

```bash
bash <(curl -Ls https://raw.githubusercontent.com/sprov065/x-ui/master/install.sh) 0.2.0
```
