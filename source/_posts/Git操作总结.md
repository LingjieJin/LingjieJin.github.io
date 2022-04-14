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