---
title: GitHub Actions 实现 Hexo 自动部署
cover: https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/actions.png
date: 2021-04-21
tags: [Hexo, GitHub]
categories: 折腾
---

### Hexo
首先我们先要在本地确保 Hexo 是可以正确运行的，比如：

```bash
hexo clean
hexo deploy
```

确认 _config.yml 文件中有类似如下的 GitHub Pages 配置：

```yaml
deploy:
  type: git
  repository: https://github.com/wejudging/wejudging.github.io.git
  branch: main
```


> 注意：请将 repository 修改为你自己的仓库地址。

### 生成秘钥
```bash
ssh-keygen -t rsa -b 4096 -C "Hexo Deploy Key" -f github-deploy-key -N ""
```

当前目录生成两个文件：
- github-deploy-key —— 私钥
- github-deploy-key.pub —— 公钥

### GitHub配置秘钥

**把私钥放到我们存放 Hexo 原始文件的代码仓库里面，用于触发 Actions 时使用;**
**把公钥放到 GitHub Pages 对应的代码仓库里面，用于 Hexo 部署时的写入工作**

#### 配置私钥
- 首先在 GitHub 上打开保存 Hexo 的仓库，访问 Settings -> Secrets，然后选择 New secret;
- 名字部分填写：HEXO_DEPLOY_KEY，注意大小写，这个后面的 GitHub Actions Workflow 要用到;
- 在 Value 的部分填入 github-deploy-key 中的内容。

#### 添加公钥

- 接下来我们需要访问存放网页的仓库，也就是 Hexo 部署以后的仓库，访问 Settings -> Deploy keys;
- 按 Add deploy key 来添加一个新的公钥；
- 在 Title中输入：HEXO_DEPLOY_PUB 字样，当然也可以填写其它自定义的名字;
- 在 Key 中粘贴 github-deploy-key.pub文件的内容。

> 注意：一定要勾选 Allow write access 来打开写权限，否则无法写入会导致部署失败。

### 创建 Workflow
**在 Hexo 的仓库中创建一个新文件：.github/workflows/auto_deploy.yml，文件的内容如下:**
```yaml
name: auto deploy # workflow name

on:
  [push] # 触发事件

jobs:
  build: # job1 id
    runs-on: ubuntu-latest # 运行环境为最新版 Ubuntu
    name: auto deploy
    steps:
    - name: Checkout # step1 获取源码
      uses: actions/checkout@v1 # 使用 actions/checkout@v1
      with: # 条件
        submodules: true # Checkout private submodules(themes or something else). 当有子模块时切换分支？
    - name: Setup Node.js 10.x
      uses: actions/setup-node@master
      with:
        node-version: "10.x"
    - name: Generate Public Files
      run: |
        npm i
        npm install hexo-cli -g
        hexo clean && hexo generate
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        deploy_key: ${{ secrets.HEXO_DEPLOY_KEY }}
        external_repository: wejudging/wejudging.github.io
        publish_branch: public
        publish_dir: ./public
        commit_message: ${{ github.event.head_commit.message }}
        user_name: 'github-actions[bot]'
        user_email: 'github-actions[bot]@users.noreply.github.com'
```

### 总结
**以上就是 GitHub Actions 自动部署 Hexo 到 GitHub Pages 的方法。**





