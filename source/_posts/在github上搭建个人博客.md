---
title: 搭建node hexo环境
date: 2022-04
tags: node, hexo
---

# 搭建node hexo环境


## WSL下安装nvm，node环境
[参考微软文档](https://docs.microsoft.com/zh-cn/windows/dev-environment/javascript/nodejs-on-wsl)
### 安装nvm node npm
1. 打开 Ubuntu 命令行（或所选的发行版）。

2. 使用以下命令安装 cURL（用于在命令行中从 Internet 下载内容的工具）：sudo apt-get install curl

3. 使用以下命令安装 nvm：
        
        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

    如果无法下载（出现curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused），可以尝试网页直接访问 **https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh** 然后复制sh内容，本地执行安装。

    或者可以参考这篇文章[如何解决类似 curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused 的问题](https://github.com/hawtim/hawtim.github.io/issues/10)

4. 若要验证安装，请输入：command -v nvm。此命令应返回“nvm”，如果你收到“找不到命令”或根本没有响应，请关闭当前终端，将其重新打开，然后重试。 在 nvm github 存储库中了解详细信息。

5. 列出当前安装的 Node 版本（此时应为无）：nvm ls

6. 安装 Node.js 的当前版本和稳定的 LTS 版本。 后面的步骤将介绍如何使用 nvm 命令在 Node.js 的活动版本之间切换。

    - 安装 Node.js 的当前稳定的 LTS 版本（推荐用于生产应用程序）：nvm install --lts

    - 安装 Node.js 的当前版本（用于测试最新的 Node.js 功能和改进，但更容易出现问题）：nvm install node

7. 列出安装的 Node 版本：nvm ls。现在应会看到刚安装的两个版本。

8. 使用以下命令验证 Node.js 是否已安装，以及是否为当前默认版本：node --version。 然后使用以下命令验证是否也有 npm：npm --version（还可以使用 which node 或 which npm 来查看用于默认版本的路径）。

9. 若要更改要用于项目的 Node.js 版本，请创建新的项目目录 mkdir NodeTest，输入目录 cd NodeTest，然后输入 nvm use node 切换到当前版本，或输入 nvm use --lts 切换到 LTS 版本。 你还可以使用已安装的任何其他版本的特定数量，如 nvm use v8.2.1。 （若要列出 Node.js 的所有可用版本，请使用以下命令：nvm ls-remote）。

### 替代版本管理器
虽然 nvm 目前是最常用的节点版本管理器，但需要考虑一些替代版本管理器：

- n 是长期存在的 nvm 替代方法，该方法使用略微不同的命令来完成相同的操作，并通过 npm 而不是 bash 脚本来安装。
- fnm 是较新的版本管理器，它声称比 nvm 快得多。 （它还使用 Azure 管道。）
- Volta 是来自 LinkedIn 团队的新版本管理器，它声称改进了速度和跨平台支持。
- asdf-vm 是适用于多种语言的单个 CLI，例如将 ike gvm、nvm、rbenv & pyenv（等）整合在一起。
nvs（Node 版本切换器）是跨平台的 nvm 替代方法，可与 VS Code 集成。

---
## Ubuntu 1804下升级Node.js
由于ubuntu1804使用默认安装方式安装的nodejs是8.0版本，hexo无法正常使用，需要升级nodejs。

### 升级方法

        curl-sL https://deb.nodesource.com/setup_10.x-o nodesource_setup.sh
        sudo bash nodesource setup.sh
        sudo apt install nodejs
        
        # 需要从源代码编译代码的NP州包
        sudo apt install build-essential

### 查看版本号

        node -v
        npm -v

---
## 安装Hexo

### 安装
    npm install hexo -g

### 升级
    npm update hexo -g

### 初始化hexo框架
    必须在空文件夹下执行
    hexo init 
    hexo init "filename"

### 创建新博客文章
    hexo n "name" == hexo new "name"

### 生成
    hexo g == hexo generate

### 启动本地服务预览
    hexo s == hexo server

### 部署
    hexo d == hexo deploy

### Hexo会监视文件变动并自动更新，无须重启服务器
    hexo server

### 静态模式
    hexo server -s 
    hexo server -p 5000 #更改端口
    hexo server -i 192.168.1.1 #自定义 IP

### 清除缓存，若是网页正常情况下可以忽略这条命令
    hexo clean 
