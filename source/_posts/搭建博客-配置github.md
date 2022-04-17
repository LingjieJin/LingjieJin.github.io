---
title: Build Blog with Hexo+Github
date: 2022-01
tags: [blog, hexo]
---

本文章的写作参考了以下：
参考[zhihu GitHub+Hexo 搭建个人网站详细教程]：https://zhuanlan.zhihu.com/p/26625249


# 准备工作 #

1. 创建一个github账号，并在github上创建一个resp
2. 下载Git
3. 设置ssh key，绑定本机和github
4. 下载node.js

# 创建Blog #
当你完成以上的准备工作后，就可以直接创建一个Blog并上传到github上。
以下的操作过程仅在windows 10环境下完成，仅供参考。

1. 创建一个空文件夹，比如Blog，打开文件夹，打开shell。
2. 初始化hexo
   ```bash
   hexo init
   ```
3. 创建一个新博客,并在本地测试预览效果
   ```bash
   # 生成博客
   hexo new first_blog
   # 生成文件
   hexo g
   # 打开本地预览
   hexo s
   # 打开浏览器，输入localhost:4000就能看到本地预览效果
   ```
4. 将本地博客推送到github端
   
   推送本地博客文件需要修改一些配置。在Blog目录下的_config.yml文件是站点配置文件


# 更多的设置 #

## 绑定域名
使用个人的个性化域名访问自己的博客。

首先打开你的域名管理（笔者使用的是阿里云），进入域名解析并选择增加记录。选择记录类型是CNAME，主要是设置CNAME的记录值为：github用户名.github.io，这个就是你之前的resp名字。

然后在github的resp中选择setting page选项，将custom domain改成你配置的解析值

这时候使用域名就能访问自己的博客了。

##

# 特别的问题 #

## windows下提示“在此系统上禁止运行脚本”的问题
原因是启动 Windows PowerShell 时，执行策略很可能是 Restricted（默认设置）。需要修改这个设置。
```
Restricted 执行策略不允许任何脚本运行。

AllSigned 和 RemoteSigned 执行策略可防止 Windows PowerShell 运行没有数字签名的脚本。

本主题说明如何运行所选未签名脚本（即使在执行策略为 RemoteSigned 的情况下），还说明如何对脚本进行签名以便您自己使用。

有关 Windows PowerShell 执行策略的详细信息，请参阅 about_Execution_Policy。
```
想了解 计算机上的现用执行策略，打开PowerShell 然后输入 get-executionpolicy
以管理员身份打开PowerShell 输入 set-executionpolicy remotesigned
参考：https://www.jianshu.com/p/4eaad2163567

***

## Hexo部署过程中碰到Spawn failed问题
参考：https://www.jianshu.com/p/c60ad2a33a1e
参考：https://blog.zhheo.com/p/128998ac.html

***

## Hexo部署过程中碰到ERROR Deployer not found: git问题
参考：https://blog.csdn.net/Java_Mike/article/details/96456318

***
