---
layout: wiki
wiki: shell
order: -202102
seo_title: 实用脚本集合
title: 实用脚本
cover: true
logo:
  src: /images/shell.png
  small: 120px
  large: 240px
description: 实用脚本集合
---
### Linux系统

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

#### qBittorrent for CentOS7一键安装脚本

```bash
wget https://raw.githubusercontent.com/wejudging/shell/main/qBittorrentCentOS7install.sh && chmod +x qBittorrentCentOS7install.sh
./qBittorrentCentOS7install.sh
```

##### 显示内容：

qBittorrentWEB地址：你的机器公网IP+端口8080
WEB账号：admin
WEB密码：adminadmin

##### 管理命令：
```bash
systemctl start qbittorrent
systemctl stop qbittorrent
systemctl restart qbittorrent
```
