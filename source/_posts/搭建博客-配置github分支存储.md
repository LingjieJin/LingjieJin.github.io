---
title: 配置github分支存储
date: 2022-01
tags:
---

# 配置github分支存储

1、仓库新建hexo分支
在Github的username.github.io仓库上新建一个hexo(分支名字可自定义)分支, 在下图箭头位置输入分支名字,完成创建；
2、设置默认分支
切换到该hexo分支，并在该仓库->Settings->Branches->Default branch中将默认分支设为hexo,然后点击update进行保存；
三、配置文件上传Github
该步骤需要在博客配置文件和主题配置文件所在的电脑上操作，想了解git相关命令，请移步这里
1.克隆hexo分支
1.1 在合适位置将上述新建的hexo分支克隆到本地, git clone git@github.com:Sandop/Sandop.github.io.git，克隆地址换成自己的地址；
1.2 在终端中cd进入该username.github.io文件目录,cd username.github.io；
1.3 在当前目录使用Git Bash执行git branch命令查看当前所在分支，应为新建的分支hexo
2.上传部署文件
2.1 先将本地博客的部署文件（Hexo博客项目目录下的全部文件）全部拷贝进username.github.io文件目录中去
2.2 准备将所有的文件都提交到hexo分支，提交时考虑以下注意事项：

将themes目录以内中的主题的.git目录删除（如果有），因为一个git仓库中不能包含另一个git仓库，否则提交主题文件夹会失败


可能有人会问，删除了themes目录中的.git不就不能git pull更新主题了吗，很简单，需要更新主题时在另一个地方git clone下来该主题的最新版本，然后将内容拷到当前主题目录即可

2.3 最后用终端或者管理工具将所有文件提交到hexo分支,命令git add .、git commit -m "first commit hexo branch"（引号内容可改）、git push;
2.4 master分支和hexo分支各自保存着一个版本。master分支用于保存博客静态资源，提供博客页面供人访问；hexo分支用于备份博客部署文件，供自己维护更新，两者在一个GitHub仓库内也不会有任何冲突
四、同步到其他电脑


将新电脑的生成的ssh key添加到GitHub账户上；


在新电脑上克隆username.github.io仓库的hexo分支到本地，此时本地git仓库处于hexo分支；


切换到username.github.io目录，执行npm install(由于仓库有一个.gitignore文件，里面默认是忽略掉 node_modules文件夹的，也就是说仓库的hexo分支并没有存储该目录，所以需要install下)；


在新电脑上安装hexo命令,npm install -g hexo；


到这里了就可以开始在自己的新电脑上写博客了！

5.1 编辑、撰写文章或其他博客更新改动


5.2 依次执行git add .、git commit -m '***'（引号内容为描述提交内容）、git push指令，保证xxx分支版本最新


5.3 执行hexo clean && hexo g && hexo d指令，完成后就会发现，最新改动已经更新到master分支了，两个分支互不干扰！



每次换电脑更新博客的时候, 在修改之前最好也要git pull拉取一下最新的更新

作者：Sandop
链接：https://juejin.cn/post/6844903780664737806
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。