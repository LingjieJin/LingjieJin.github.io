---
title: hexo introduction
date: 2022-01-22 17:27:34
tags: hexo
---

# Hexo 框架的使用方法 #

## Hexo命令 ##
以下是hexo的命令：
```bash
# 安装
npm install hexo -g

# 升级
npm update hexo -g

# 初始化hexo框架
# 必须在空文件夹下执行
hexo init 
hexo init "filename"

# 创建新博客文章
hexo n "name" == hexo new "name"

# 生成
hexo g == hexo generate

# 启动本地服务预览
hexo s == hexo server

# 部署
hexo d == hexo deploy

# Hexo会监视文件变动并自动更新，无须重启服务器
hexo server

# 静态模式
hexo server -s 
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP

# 清除缓存，若是网页正常情况下可以忽略这条命令
hexo clean 
```