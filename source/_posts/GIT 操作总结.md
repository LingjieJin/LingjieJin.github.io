---
title: GIT 操作总结
---

# GIT 操作总结

---
## 安装git服务器
https://blog.csdn.net/zc_ad/article/details/84953039

---
## 如何将本地已有仓储上传到新仓储
将已有的库重新上传到git服务器上

1. 在服务器上重新创建一个git仓储

2. 本地git文件增加一个remoteurl

        git remote add tagname git@192.168.2.199:/home/git/nvr.git
        git push --set-upstream nanjing-server master
        git push tagname

---
## 设置git分组

---
## git账户shell登录
使用git-shell

---
## 在ubuntu登录界面隐藏git账户
1. 首先跳转到以下目录
    
        cd /var/lib/AccountsService/users/

2. ls后可以看到你想要隐藏的某个用户文件

3. 然后打开该文件
    
        vim username

4. 修改
    
        [User]
        XSession=
        SystemAccount=false  //将false改为true


---
## GIT 推送代码到远程新分支
        git push origin 本地分支:远端希望创建的分支

---
## GIT 创建本地新分支并推送到远程
### 1. 切换到需要基于变更的主分支
        git checkout master

### 2. 创建本地新分支并切换至新分支
        git checkout -b new_branch

### 3. 将新建分支推送至远程仓库
        git push origin new_branch:new_branch

### 4.解决报错：当前分支没有跟踪信息。（There is no tracking information for the current branch.）
        git branch --set-upstream-to=origin/new_branch

        # 取消跟踪
        git branch --unset-upstream master
　　
### 5. 删除本地旧的分支，同时删除远程旧的分支
#### 删除远程分支： 
        git push origin --delete 分支名字

#### 删除本地分支： 
        git branch -D 分支名字