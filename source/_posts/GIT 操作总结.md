---
title: GIT 操作总结
date: 2022-05
tags: [git]
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

-------------------------------------------------------------------------------------

## git 拉取远程分支

1. *在Git Bash中直接输入指令：git clone -b dev 代码仓库地址 （dev是分支名称）

2. 
*与远程代码仓库建立连接：git remote add origin 代码仓库地址

*将远程分支拉到本地：git fetch origin dev（dev即分支名）

*创建本地分支：git checkout -b LocalDev origin/dev (LocalDev 为本地分支名，dev为远程分支名)

*最后一步将远程分支拉取到本地：git pull origin dev（dev为远程分支名）

## 使用git fetch
1.初始化git
    git init

2.获取远程分支
    git fetch url remote-branch:local-branch
    此时本地只有该分支，然后使用git checkout 切换到那个分支

git pull git@github.com:LingjieJin/SomeSample.git main:localmain

---------------------------------------------------------------------------------------

## git 切换到指定远程分支

### 查看远程所有分支
    git branch -a

### 新建分支并切换到指定分支
    git checkout -b 本地分支名 origin/远程分支名

### 查看本地分支及追踪的分支
    git branch -vv

### 将本地分支推送到远程
    git push <远程主机名> <本地分支名>:<远程分支名>

### 创建一个空白的分支
    git checkout --orphan newbranch

-------------------------------------------------------------------------------------------

## git删除记录中的大文件

### 1.查看git仓库大小
        git count-objects -v

### 2.查找
        git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -5

### 3.找到对应的文件名（包括目录结构）
        git rev-list --objects --all | grep 8f10eff91bb6aa2de1f5d096ee2e1687b0eab007

        git filter-branch --index-filter 'git rm --cached --ignore-unmatch <your-file-name>'
        rm -rf .git/refs/original/
        git reflog expire --expire=now --all
        git fsck --full --unreachable
        git repack -A -d
        git gc --aggressive --prune=now
        git push --force [remote] master

里面最重要的两条命令是 git filter-branch 和 gc, filter-branch 真正在清理，但是只运行它也是没用的，需要再删除备份的文件，重新打包之类的，最后的gc命令

用来收集产生的垃圾，最终清除大文件

如果真的要完全把这个对象删除，可以运行 git prune 命令

-------------------------------------------------------------------------------------------

## Git HEAD detached from XXX 解决办法

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

----------------------------------------------------------------------------------------

## 使用 .gitignore文件配置git上传忽略

## 配置参考
[GitHub gitignore](https://github.com/github/gitignore/, "github 推荐")

通过配置* .gitignore *文件，可以指定git管理所需要的文件，忽略代码编译过程中产生的额外文件

修改.gitignore文件中的标示的方法来忽略开发者想忽略掉的文件或目录

在.gitignore文件中的每一行保存一个匹配的规则

sample：
```bash
# 此为注释 – 将被 Git 忽略

*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt

*.log
*.temp
/temp

#               表示此为注释,将被Git忽略
*.a             表示忽略所有 .a 结尾的文件
!lib.a          表示但lib.a除外
/TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt

bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
/bin:           表示忽略根目录下的bin文件
/*.c:           表示忽略cat.c，不忽略 build/cat.c
debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
**/foo:         表示忽略/foo,a/foo,a/b/foo等
a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
!/bin/run.sh    表示不忽略bin目录下的run.sh文件
*.log:          表示忽略所有 .log 文件
config.php:     表示忽略当前路径的 config.php 文件

/mtk/           表示过滤整个文件夹
*.zip           表示过滤所有.zip文件
/mtk/do.c       表示过滤某个具体文件

需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
!*.zip
!/mtk/one.txt

还有一些规则如下：
fd1/*
说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

/fd1/*
说明：忽略根目录下的 /fd1/ 目录的全部内容；

/*
!.gitignore
!/fw/
/fw/*
!/fw/bin/
!/fw/sf/
说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。
```

## gitignore文件忽略规则的优先级

1）从命令行中读取可用的忽略规则
2）当前目录定义的规则
3）父级目录定义的规则，依次递推
4）$GIT_DIR/info/exclude 文件中定义的规则
5）core.excludesfile中定义的全局规则

## 删除旧版本的文件
新建的文件在git中会有缓存
如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的
这时候我们就应该先把本地缓存删除，然后再进行git的push，这样就不会出现忽略的文件了
git清除本地缓存命令如下

```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

--------------------------------------------------------------------------------------------

## git clone 指定的文件

## example

> 现在有一个test仓库 https://github.com/mygithub/test
你要git clone里面的tt子目录：
在本地的硬盘位置打开Git Bash

```bash
# 创建一个空的本地仓库，同时将远程Git Server URL加入到Git Config文件中
mkdir project_folder
​
cd project_folder
​
git init
​
git remote add -f origin <url>

# 在Config中允许使用Sparse Checkout模式
git config core.sparsecheckout true

# 接下来你需要告诉Git哪些文件或者文件夹是你真正想Check Out的
# 你可以将它们作为一个列表保存在
# .git/info/sparse-checkout
# 文件中
echo “libs” >> .git/info/sparse-checkout

echo “apps/register.go” >> .git/info/sparse-checkout

echo “resource/css” >> .git/info/sparse-checkout

# 以正常方式从你想要的分支中将你的项目拉下来就可以了
git pull origin master

# 总结
1.指定远程仓库
2.指定克隆模式: 稀疏克隆模式
3.指定克隆的文件夹(或者文件)
4.拉取远程文件

```

-------------------------------------------------------------------------------------------