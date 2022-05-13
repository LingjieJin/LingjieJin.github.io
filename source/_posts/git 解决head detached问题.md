---
title: Git HEAD detached from XXX 解决办法
---

# Git HEAD detached from XXX 解决办法


### 解决办法

#### 1. 查看当前分支状态
    git branch
    * (HEAD detached at 925fda6)
    master

#### 2.新建一个临时 tem 分支，把当前提交的代码放到整个分支
    git branch tem
    git checkout tem

#### 3.换回要回到的那个分支，这里是 master
    git checkout master

#### 4.merge 刚才创建的临时分支
    git merge tem

#### 5.检查是否有冲突，没有就提交到远端
    git push origin master

#### 6.删除临时分支
    git branch -d tem

### GIT HEAD游离状态
#### HEAD 游离状态的利与弊
Git 中的 HEAD 可以理解为一个指针，我们可以在命令行中输入 cat .git/HEAD 查看当前 HEAD 指向哪儿，一般它指向当前工作目录所在分支的最新提交。

当使用 git checkout < branch_name> 切换分支时，HEAD 会移动到指定分支。

但是如果使用的是 git checkout < commit id>，即切换到指定的某一次提交，HEAD 就会处于 detached 状态（游离状态）。

HEAD 处于游离状态时，我们可以很方便地在历史版本之间互相切换，比如需要回到某次提交，直接 checkout 对应的 commit id 或者 tag 名即可。

它的弊端就是：在这个基础上的提交会新开一个匿名分支！

也就是说我们的提交是无法可见保存的，一旦切到别的分支，游离状态以后的提交就不可追溯了。

