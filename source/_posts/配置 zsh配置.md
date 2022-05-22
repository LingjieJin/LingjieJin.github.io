---
title: zsh 配置
date: 2022-05
tags: [zsh, linux, mac]
---

# zsh配置
记录各个版本的zsh安装和配置

# Mac下安装zsh以及配置
[Mac 安装zsh参考](https://www.xiebruce.top/590.html)

### 安装zsh
    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

### 卸载oh-my-zsh命令
    uninstall_oh_my_zsh

### 使用homebrew安装
    brew install zsh

### 安装插件
#### 安装meslo字体
应用这个主题需要特殊的字体支持，否则会出现乱码情况，这时我们来配置字体：
安装 Meslo 字体库。
方法1、可以直接复制下面命令到终端中安装：
```
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```
应用字体到iTerm2下，我自己喜欢将字号设置为16px，看着舒服（iTerm -> Preferences -> Profiles -> Text -> Change Font）
重新打开iTerm2窗口，这时便可以看到效果了

#### 使用homebrew安装语法高亮
brew install zsh-syntax-highlighting
配置.zshrc文件，插入一行。
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source ~/.zshrc

# 安装coreutils
brew install coreutils

------------------------------------------------------------------------------------------------------

# Linux下安装zsh
## 安装zsh
检查是否已经安装了zsh，输入 zsh --version 查看版本信息
执行 sudo apt-get install zsh

## 修改默认shell
查看当前默认使用的shell：

    echo $SHELL
    或者
    echo $0

    查看当前有哪些shell：
    cat /etc/shells


切换shell为zsh：
    
    chsh -s /bin/zsh


## 安装oh-my-zsh插件
### 下载安装脚本
    wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

    或者
    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

    国内服务器如果用上边的命令安装不了，可以用gitee同步的仓库来安装
    sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"

### 由于国内下载git源可能会很慢，可以修改成gitee下载：
vim install.sh：
找到以下部分：

    # Default settings
    ZSH=${ZSH:-~/.oh-my-zsh}
    REPO=${REPO:-ohmyzsh/ohmyzsh}
    REMOTE=${REMOTE:-https://github.com/${REPO}.git}
    BRANCH=${BRANCH:-master}

然后将中间两行改为：

    REPO=${REPO:-mirrors/oh-my-zsh}
    REMOTE=${REMOTE:-https://gitee.com/${REPO}.git}

### 配置zsh
修改 ~/.zshrc 文件
#### 修改主题：
ZSH_THEME="robbyrussell"
改为
ZSH_THEME="ys"
#### 随机主题
ZSH_THEME="random" 

从你最喜欢的主题列表中选择随机主题

    ZSH_THEME_RANDOM_CANDIDATES=(
    "robbyrussell"
    "agnoster"
    )

#### 修改插件：
plugins=(git)
改为
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
- Git：用于在你的主机名后显示git项目信息，比如分支，目录，当前项目状态等信息，可以使用各种git命令缩写；
- z：用于目录间快速跳转，比如之前进入过~/User/my_project目录后，下一次再想进入的时候，直接z my_project即可，对于较长目录跳转非常的实用；
- zsh-syntax-highlighting：用于高亮显示常见的命令，比如ls，cd等命令为绿色，输入错误命令时会显示红色；
- zsh-autosuggestions：当你输入命令的时候，会用灰色显示出你可能想输入的推荐命令，直接键盘→就能补全命令，效率神器。

#### 如果.bash_profile中有配置内容的话，还需要增加一行：
source ~/.bash_profile

### 安装插件
#### 安装zsh-syntax-highlighting
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

    // 或者用git下载
    git clone git@github.com:zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

#### 安装zsh-autosuggestions
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

    // 或者用git下载
    git clone git@github.com:zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

#### 安装powerline插件
    sudo apt install powerline fonts-powerline
    sudo apt install zsh-theme-powerlevel9k
    echo "source /usr/share/powerlevel9k/powerlevel9k.zsh-theme" >> ~/.zshrc

#### 安装完成后，重新更新配置
    source .zshrc


# zsh配置

## 别名配置
nano ~/.zshrc
```bash
# For git
alias gs="git status"
alias ga='git add'
alias gd='git diff'
alias gf='git fetch'
alias grv='git remote -v'
alias gbr='git branch'
alias gpl="git pull"
alias gps="git push"
alias gco="git checkout"
alias gl="git log"
alias gc="git commit -m"
alias gm="git merge"

# For local
alias cd..="cd .."
alias cd...="cd ../.."
alias cd....="cd ../../.."
alias ..="cd .."
alias ...="cd ../.."
alias ....="cd ../../.."
alias ip="curl ip.cn"
```

# 隐藏用户名
在zshrc中添加该字段：

DEFAULT_USER="用户名"

用户名指当前电脑用户，可以用whoami查看

### 安装powerline
先安装pip
sudo easy_install pip
再安装Powerline
pip install powerline-status



## 