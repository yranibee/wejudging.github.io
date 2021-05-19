---
layout: wiki
wiki: onedrive-cf-index
order: -202102
seo_title: 由 CloudFlare Workers 强力驱动的 OneDrive 索引
title: onedrive-cf-index
cover: true
logo:
  src: https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/onedrive-cf-index.png
  small: 120px
  large: 240px
description: 由 CloudFlare Workers 强力驱动的 OneDrive 索引
---

在线演示：[onedrive.weijiajin.com](https://onedrive.weijiajin.com/).

## 功能

### 🚀 功能一览

- 全新「面包屑」导航栏；
- 令牌凭证由 Cloudflare Workers 自动刷新，并保存于（免费的）全局 KV 存储中；
- 使用 [Turbolinks®](https://github.com/turbolinks/turbolinks) 实现路由懒加载；
- 支持由世纪互联运营的 OneDrive 版本；
- 支持 SharePoint 部署；

### 🗃️ 目录索引显示

- 全新支持自定义的设计风格：spencer.css；
- 支持使用 Emoji 作为文件夹图标（如果文件夹名称第一位是 Emoji 则自动开启该功能）；
- 渲染 README.md 如果当前目录下包含此文件，使用 [github-markdown-css](https://github.com/sindresorhus/github-markdown-css) 渲染样式；
- 支持「分页」，没有一个目录仅限显示 200 个项目的限制了！

### 📁 文件在线预览

- 根据文件类型渲染文件图标，图标使用 [Font Awesome icons](https://fontawesome.com/)；
- 支持预览：
  - 纯文本：.txt ...
  - Markdown 格式文本：.md , .mdown , .markdown ...
  - 图片（支持 Medium 风格的图片缩放）： .png , .jpg , and .gif ...
  - 代码高亮： .js , .py , .c ,  .json ...
  - PDF（支持懒加载、加载进度、Chrome 内置 PDF 阅读器）： .pdf ...
  - 音乐： .mp3 , .aac , .wav , .oga ...
  - 视频： .mp4 , .flv , .webm , .m3u8 ...

### 🔒 私有文件夹

我们可以给某个特定的文件夹（目录）上锁，需要认证才能访问。我们可以在 src/auth/config.js  文件中将我们想要设为私有文件夹的目录写入 ENABLE_PATHS 列表中。我们还可以自定义认证所使用的用户名 NAME 以及密码，其中认证密码保存于 AUTH_PASSWORD 环境变量中，需要使用 wrangler 来设置这一环境变量：

```bash
wrangler secret put AUTH_PASSWORD
# 在这里输入你自己的认证密码
```

有关 wrangler 的使用细节等详细内容，请参考 [接下来的部分段落](#准备工作)。

### ⬇️ 代理下载文件 / 文件直链访问

- [可选] Proxied download（代理下载文件）：?proxied - 经由 CloudFlare Workers 下载文件，如果（1）config/default.js 中的 proxyDownload 为 true，以及（2）使用参数 ?proxied 请求文件；
- [可选] Raw file download（文件直链访问）：?raw - 返回文件直链而不是预览界面；
- 两个参数可以一起使用，即 ?proxied&raw 和 ?raw&proxied 均有效。

### 其他功能

请参考原项目的「🔥 新特性 V1.1」部分：[onedrive-index-cloudflare-worker](https://github.com/heymind/OneDrive-Index-Cloudflare-Worker#-%E6%96%B0%E7%89%B9%E6%80%A7-v11)，**但我不保证全部功能均可用，因为本项目改动部分很大。**

## 部署指南

_又臭又长的中文版部署指南预警！_

### 生成 OneDrive API 令牌

1. 访问此 URL 创建新的 Blade app：[Microsoft Azure App registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)（普通版 OneDrive）或 [Microsoft Azure.cn App registrations](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)（OneDrive 世纪互联版本），**建议将语言设置为「英语」以保证以下步骤中提到的模块和按钮的名称一致**：

   1. 使用你的 Microsoft 账户登录，选择 New registration ；
   2. 在 Name 处设置 Blade app 的名称，比如 my-onedrive-cf-index ；
   3. 将 Supported account types 设置为 Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox) 。OneDrive 世纪互联用户设置为：任何组织目录（任何 Azure AD 目录 - 多租户）中的帐户；
   4. 将 Redirect URI (optional) 设置为 Web（下拉选项框）以及 http://localhost ；
   5. 点击 Register.

   ![](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/项目图片/onedrive-cf-index/assets/register-app.png)

2. 在 Overview 面板获取你的 Application (client) ID - client_id：

   ![](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/项目图片/onedrive-cf-index/assets/client-id.png)

3. 打开 Certificates & secrets 面板，点击 New client secret，创建一个新的叫做 client_secret 的 Client secret，并将 Expires 设置为 Never。点击 Add 并复制 client_secret 的 Value 并保存下来 **（仅有此一次机会）**：

   ![](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/项目图片/onedrive-cf-index/assets/add-client-secret.png)

4. 打开 API permissions 面板，选择 Microsoft Graph，选择 Delegated permissions，并搜索 offline_access, Files.Read, Files.Read.All 这三个权限，**选择这三个权限，并点击 Add permissions：**

   ![](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/项目图片/onedrive-cf-index/assets/add-permissions.png)

   你应该成功开启这三个权限：

   ![](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/项目图片/onedrive-cf-index/assets/permissions-used.png)

5. 获取 refresh_token，在本机（需要 Node.js 和 npm 环境，安装和推荐配置请参考 [准备工作](#准备工作)）上面执行如下命令：

   ```bash
   npx @beetcb/ms-graph-cli
   ```
- 选择onedrive
- 输入 ID 与 密码
- 默认 http://localhost
- **复制终端的地址粘贴到浏览器（mac终端有个bug会出现几个换行，要手动删除变成完整的链接）**
- 授权后 复制链接里面的 refresh_token

根据你自己的情况选择合适的选项，并输入我们上面获取到的一系列 token 令牌配置等，其中 redirect_url 可以直接设置为 http://localhost。

6. 最后，在我们的 OneDrive 中创建一个公共分享文件夹，比如 /Public 即可。建议不要直接分享根目录!

最后，这么折腾完，我们应该成功拿到如下的几个凭证：

- refresh_token
- client_id
- client_secret
- redirect_uri
- base：默认为 /Public。

_是，我知道很麻烦，但是这是微软，大家理解一下。🤷🏼‍♂️_

### 准备工作

Fork 再 clone 或者直接 clone 本仓库，并安装依赖 Node.js、npm 以及 wrangler。

建议使用 homebrew 安装配置环境 

```bash
# 安装 CloudFlare Workers 官方编译部署工具
sudo npm i @cloudflare/wrangler -g

# 使用 npm 安装依赖
npm install

# 使用 wrangler 登录 CloudFlare 账户
wrangler login

# 使用这一命令检查自己的登录状态
wrangler whoami
```

打开 <https://dash.cloudflare.com/login> 登录 CloudFlare，选择自己的域名，**再向下滚动一点，我们就能看到右侧栏处我们的 account_id 以及 zone_id 了。** 同时，在 Workers -> Manage Workers -> Create a Worker 处创建一个 **onerive** worker。

修改我们的 wrangler.toml ：

- name：就是我们刚刚创建的 onedrive worker 名称，我们的 Worker 默认会发布到这一域名下：onedrive.weijiajin.workers.dev ；
- account_id ：我们的 Cloudflare Account ID；
- zone_id ：我们的 Cloudflare Zone ID。

创建叫做 BUCKET 的 Cloudflare Workers KV bucket：

```bash
# 创建 KV bucket
wrangler kv:namespace create "BUCKET"

# ... 或者，创建包括预览功能的 KV bucket
wrangler kv:namespace create "BUCKET" --preview
```

修改 wrangler.toml 里面的 kv_namespaces ：

- kv_namespaces ：我们的 Cloudflare KV namespace，仅需替换 id 和（或者） preview_id 即可。
- 如果不需要预览功能，那么移除 preview_id 即可。

修改 src/config/default.js ：

- client_id ：刚刚获取的 OneDrive client_id ；
- base ：之前创建的 base 目录；
- 如果你部署常规国际版 OneDrive，那么忽略以下步骤即可；(我的是这种)
- 如果你部署的是由世纪互联运营的中国版 OneDrive：
  - 修改 type 下的 accountType 为 1 ；
  - 保持 driveType 不变；
- 如果你部署的是 SharePoint 服务：
  - 保持 accountType 不变；
  - 修改 driveType 下的 type 为 1 ；
  - 并根据你的 SharePoint 服务修改 hostName 和 sitePath 。

使用 wrangler 添加 Cloudflare Workers 环境变量 ：

```bash
# 添加我们的 refresh_token 和 client_secret（macos的终端可以粘贴不了那么多，可以用cloudflare的网页端操作）
wrangler secret put REFRESH_TOKEN
# ... 并在这里粘贴我们的 refresh_token

wrangler secret put CLIENT_SECRET
# ... 并在这里粘贴我们的 client_secret

wrangler secret put AUTH_PASSWORD
# ... 在这里输入我们自己设置的认证密码
```

### 编译与部署

我们可以使用 wrangler 预览部署：

```bash
wrangler preview
```

如果一切顺利，我们即可使用如下命令发布 Cloudflare Worker：

```bash
wrangler publish
```


### GitHub Actions 自动部署

我们也可以创建一个 GitHub Actions 来在每次  push  到 GitHub 仓库时自动发布新的 Worker，详情参考：[main.yml](https://raw.githubusercontent.com/wejudzaiging/onedrive-cf-index/main/.github/workflows/main.yml)。

- secrets.CF_API_TOKEN 是在 GitHub 项目设置的 secrets 里设置（只需要设置这一个环境变量就可以了）
- CF_API_TOKEN 的值  是在执行  wrangler login 这个命令后存储在自己的电脑
- 示例登录授权api成功后，我是这样获取的：
```bash
vi /Users/weijiajin/.wrangler/config/default.toml
```

### 域名下部署

如果想在自己的域名下部署 Cloudflare Worker，请参考：[How to Setup Cloudflare Workers on a Custom Domain](https://www.andressevilla.com/how-to-setup-cloudflare-workers-on-a-custom-domain/)。

## 样式、内容的自定义

- 我们 **应该** 更改默认「着落页面」，直接修改 src/folderView.js 中 `intro` 的 HTML 即可；
- 我们也 **应该** 更改页面的 header，直接修改 src/render/htmlWrapper.js 即可；
- 样式 CSS 文件位于 themes/spencer.css，可以根据自己需要自定义此文件，同时也需要更新 src/render/htmlWrapper.js  文件中的 commit HASH；
- 我们还可以自定义 Markdown 渲染 CSS 样式、PrismJS 代码高亮样式，等等等。

---

🏵 **Authored and maintained by Spencer Woo.**




[![Hosted on Cloudflare Workers](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/CF-Workers-f38020.svg)](https://workers.cloudflare.com/) 


[![Deploy](https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/Github-Actions-f38020.svg)](https://github.com/wejudging/onedrive-cf-index/actions)



