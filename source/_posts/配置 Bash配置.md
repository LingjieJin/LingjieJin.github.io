---
title: Bash配置
date: 2022-05
tags: [bash, linux, mac]
---

# Linux Bash 配置


### BASH PS1配置
```sh
   PS1='[\[\e[01;05;32m\]\u\[\e[00m\]@\[\e[01;33m\]\h\[\e[00m\]:\[\e[01;34m\]\w\[\e[00m\]]\$ '

   PS1="Time:\[\033[1;35m\]\T     \[\033[0m\]User:\[\033[1;33m\]\u     \[\033[0m\]Dir:\[\033[1;32m\]\w\[\033[0m\]\n\$"

   颜色配置:
   \[\033[1;31m\]
   底线：ANSI 色彩控制语法。\033 声明了转义序列的开始，然后是 [ 开始定义颜色。
   
   第一组数字：亮度 (普通0, 高亮度1, 闪烁2)。
   第二组数字：顏色代码。
   颜色: 30=black 31=red 32=green 33=yellow 34=blue 35=magenta 36=cyan 37=white
   
   \[\033[0m\]
   关闭 ANSI 色彩控制，通常置于尾端。

   显示内容配置:

   \a     ASCII响铃字符 (07)
   \d     “周 月 日”格式的日期
   \D{format}   参数format被传递给strftime(3)来构造自定格式的时间并插入提示符中；该参数为空时根据本地化设置自动生成格式。
   \e     ASCII转义字符（ESC) (033)
   \h     主机名在第一个点号前的内容
   \H     完全主机名
   \j     shell当前管理的任务数
   \l     shell终端设备的基本名称
   \n     新行
   \r     回车
   \s     shell的名称，$0的基本名称
   \t     当前时间（24小时） HH:MM:SS
   \T     当前时间（12小时） HH:MM:SS
   \@     当前时间（12小时） am/pm
   \A     当前时间（24小时） HH:MM
   \u     当前用户名称
   \v     bash版本(如"2.00")
   \V     bash版本+补丁号(如"2.00.0")
   \w     当前工作目录
   \W     当前工作目录的基本名称
   \!     该命令的历史数（在历史文件中的位置）
   \#     该命令的命令数（当前shell中执行的序列位置）
   \$     根用户为"#"，其它用户为"$"
   \nnn   8进制数
   \\     反斜杠
   \[     表示跟在后面的是非打印字符，可用于shell的颜色控制
   \]     表示非打印字符结束
```

---
### 启动新的bash配置

      echo "export PS1="Time:\[33[1;35m\]\T \[33[0m\]User:\[33[1;33m\]\u \[33[0m\]Dir:\[33[1;32m\]\w\[33[0m\]\n\$" " >> .bashrc

      source .bashrc

---
### sample
```bash
   # print the git branch name if in a git project
   parse_git_branch() {
      git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)//'
   }

   # set  the input prompt symbol
   ARROW="❯"

   # define text  formatting
   PROMPT_BOLD="$(tput bold)"
   PROMPT_UNDERLINE="$(tput  smul)"
   PROMPT_FG_GREEN="$(tput setaf 2)"
   PROMPT_FG_CYAN="$(tput setaf  6)"
   PROMPT_FG_YELLOW="$(tput setaf 3)"
   PROMPT_FG_MAGENTA="$(tput setaf  5)"
   PROMPT_RESET="$(tput sgr0)"

   # save each section prompt section in  variable
   PROMPT_SECTION_SHELL="\[$PROMPT_BOLD$PROMPT_FG_GREEN\]\s\[$PROMPT_RESET\]"
   PROMPT_SECTION_DIRECTORY="\[$PROMPT_UNDERLINE$PROMPT_FG_CYAN\]\W\[$PROMPT_RESET\]"
   PROMPT_SECTION_GIT_BRANCH="\[$PROMPT_FG_YELLOW\]\`parse_git_branch\`\[$PROMPT_RESET\]"
   PROMPT_SECTION_ARROW="\[$PROMPT_FG_MAGENTA\]$ARROW\[$PROMPT_RESET\]"

   #  set the prompt string using each section variable
   PS1="🐂  $PROMPT_SECTION_SHELL 🍎  $PROMPT_SECTION_DIRECTORY 🌹  $PROMPT_SECTION_GIT_BRANCH 🌹$PROMPT_SECTION_ARROW "
```


# Mac Bash配置

## 修改bash配置
~/.bash_profile用户目录下的该文件纪录了bash的配置信息
```bash
#enables colorin the terminal bash shell export
export CLICOLOR=1

#setsup thecolor scheme for list export
export LSCOLORS=gxfxcxdxbxegedabagacad
 
#sets up theprompt color (currently a green similar to linux terminal)
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[00m\]\$  (设置几个空格，个人喜欢两个)'
#enables colorfor iTerm
export TERM=xterm-256color
```

如果希望加入git的语法高亮，参考以下:
```bash
#Git branch status
function git_status {
  local unknown untracked stash clean ahead behind staged dirty diverged
  unknown='0;34' # blue
  untracked='0;32' # green
  stash='0;32' # green
  clean='0;32' # green
  ahead='0;33' # yellow
  behind='0;33' # yellow
  staged='0;96' # cyan
  dirty='0;31' # red
  diverged='0;31' # red

  if [[ $TERM = *256color ]]; then
    unknown='38;5;20' # dark blue
    untracked='38;5;76' # mid lime-green
    stash='38;5;76' # mid lime-green
    clean='38;5;82' # brighter green
    ahead='38;5;226' # bright yellow
    behind='38;5;142' # darker yellow-orange
    staged='38;5;214' # orangey yellow
    dirty='38;5;202' # orange
    diverged='38;5;196' # red
  fi

  branch=$(git rev-parse --abbrev-ref HEAD 2>/dev/null)
  if [[ -n "$branch" ]]; then
    if [[ "$branch" == 'HEAD' ]]; then
       branch=$(git rev-parse --short HEAD 2>/dev/null)
    ## 建议修改
    ## 第一次从github 克隆下来的仓库，执行git rev-parse --short HEAD 2>/dev/null 为空，导致会显示 ()$现象
       branch=master
    fi
    git_status=$(git status 2> /dev/null)
    # If nothing changes the color, we can spot unhandled cases.
    color=$unknown
    if [[ $git_status =~ 'Untracked files' ]]; then
      color=$untracked
      branch="${branch}+"
    fi
    if git stash show &>/dev/null; then
      color=$stash
      branch="${branch}*"
    fi
    if [[ $git_status =~ 'working directory clean' ]]; then
      color=$clean
    fi
    if [[ $git_status =~ 'Your branch is ahead' ]]; then
      color=$ahead
      branch="${branch}"
    fi
    if [[ $git_status =~ 'Your branch is behind' ]]; then
      color=$behind
      branch="${branch}"
    fi
    if [[ $git_status =~ 'Changes to be committed' ]]; then
      color=$staged
    fi
    if [[ $git_status =~ 'Changed but not updated' ||
          $git_status =~ 'Changes not staged'      ||
          $git_status =~ 'Unmerged paths' ]]; then
      color=$dirty
    fi
    if [[ $git_status =~ 'Your branch'.+diverged ]]; then
      color=$diverged
      branch="${branch}!"
    fi
    printf "\033[%sm%s\033[0m" "$color" "($branch)"
  fi
  return 0
}

#bles colorin the terminal bash shell export
export CLICOLOR=1

#sets up thecolor scheme for list export
export LSCOLORS=gxfxcxdxbxegedabagacad

#sets up theprompt color (currently a green similar to linux terminal)
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[01;31m\]$(git_status)\[\033[0m\]\$ '
## 建议修改
## 小写的w 改成大写的W 是以为小写的w 显示的是当前文件夹的绝对路径，放到控制台里面贴着很长，大写的W则只会显示当前文件夹的名字
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\W\[\033[01;31m\]$(git_status)\[\033[0m\]\$ '

#enables colorfor iTerm
export TERM=xterm-256color
```
保存即可生效。

为了确保下次开机命令依然有效，执行如下命令：

      echo "[ -r ~/.bashrc ] && source ~/.bashrc" >> .bash_profile

### 配置vim
编辑 vim .vimrc 开启配置项

      #代码高亮开启
      syntax on

      #文件内容行号
      set nu