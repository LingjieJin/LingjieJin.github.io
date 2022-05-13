---
title: git 切换到指定远程分支
---

# git 切换到指定远程分支

### 查看远程所有分支
    git branch -a

### 新建分支并切换到指定分支
    git checkout -b 本地分支名 origin/远程分支名

### 查看本地分支及追踪的分支
    git branch -vv

### 将本地分支推送到远程
    git push <远程主机名> <本地分支名>:<远程分支名>