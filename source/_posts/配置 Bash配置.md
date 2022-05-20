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


# .bashrc 和.bash_profile 的区别
[bash 详细介绍](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_01_02.html)

### 
当作为交互式登录shell调用时，Bash查找/etc/profile文件，如果文件存在，它将运行文件中列出的命令。 然后Bash按照列出的顺序搜索~/.bash_profile，~/.bash_login和~/.profile文件，并从找到的第一个可读文件中执行命令。

当Bash作为交互式非登录shell程序调用时，它会从~/.bashrc中读取并执行命令（如果该文件存在并且可读）。

当Bash作为交互式登录shell调用时，将读取并执行.bash_profile

对于交互式非登录shell，则执行.bashrc。

仅应运行一次的命令应该使用.bash_profile ，例如自定义$PATH 环境变量。

将每次启动新Shell时应该运行的命令放在.bashrc文件中。 这包括您的别名和function，自定义提示，历史记录自定义等。

通常，~/.bash_profile包含以下类似于.bashrc文件来源的行。 这意味着每次您登录到终端时，都会读取并执行两个文件。
if [ -f ~/.bashrc ]; then
	. ~/.bashrc
fi

/etc/profile 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从/etc/profile.d目录的配置文件中搜集shell的设置。
/etc/bashrc 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取。
~/.bash_profile 每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次! 默认情况下,他设置一些环境变量,执行用户的.bashrc文件。
~/.bashrc 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。
~/.bash_logout 少见，但是意味着当每次退出系统(退出bash shell)时,执行该文件。

另外/etc/profile 中设定的变量(全局)的可以作用于任何用户, 而~/.bashrc 等中设定的变量(局部)只能继承 /etc/profile 中的变量,他们是"父子"关系。

profile用于登录式shell, 而bashrc用于每个交互式shell
~/.bash_profile 是交互式、login 方式进入 bash 运行的
~/.bashrc 是交互式 non-login 方式进入 bash 运行的
通常二者设置大致相同，所以通常前者会调用后者。
所以一般优先把变量设置在.bashrc里面。比如在crontab里面执行一个命令，.bashrc 设置的环境变量会生效，而 .bash_profile 不会。

nteractive login shell 的形式调用，Files read/etc/profile~/.bash_profile, ~/.bash_login or ~/.profile        （first existing readable file is read）~/.bash_logout （upon logout）interactive non-login shell 的形式调用，Files read:~/.bashrc                  //该文件通常指向 ~/.bash_profilenon-interactively， 脚本文件通常用这种方式。Files read:defined by BASH_ENVInvoked remotely,Files read:~/.bashrc



### .bash_profile 的意义
.bash_profile文件包含用于设置环境变量的命令，因此shell将继承这些变量。

在交互式登录shell中，bash首先查找 /etc/profile 文件。如果找到，bash将在当前shell中读取并执行它。结果是 /etc/profile为所有用户设置环境配置类似地，bash然后检查主目录(cd ~ 进入的目录为主目录)中是否存在 .bash_profile。

如果存在，则bash在当前shell中执行 .bash_profile，Bash然后停止寻找其他文件，如 .bash_login 和 .profile。

如果bash没有找到 .bash_profile，那么它将按照顺序查找 .bash_login 和 .profile，并只执行第一个可读的文件。

让我们研究一个示例 .bash_profile文件。这里我们重新设置并导出PATH变量
```bash
echo "Bash_profile execution starts.."  
PATH=$PATH:$HOME/bin; 
export PATH; 
echo "Bash_profile execution stops.."
```
在交互式登录shell的命令提示符之前，我们将看到下面的输出
```bash
Bash_profile execution starts.. 
Bash_profile execution stops.. 
[example@example ~]$
```

### .bashrc 的意义
.bashrc包含特定于bash shell的命令。

每个交互式非登录shell首先读取 .bashrc，通常，.bashrc是添加别名和bash相关功能的最佳场所。

bash shell在主目录中查找 .bashrc文件，并使用source在当前shell中执行它。
让我们通过样例认识 .bashrc文件
```bash
echo "Bashrc execution starts.." 
alias elui='top -c -u $USER' 
alias ll='ls -lrt' 
echo "Bashrc execution stops.."
```
在交互式非登录shell的命令提示符之前，我们将看到下面的输出
```bash
[example@example ~]$ bash
Bashrc execution starts.. 
Bashrc execution stops.. 
[example@example ~]$
```
### .profile 的意义
在交互式shell登录过程中，如果在主目录中不存在 .bash_profile，则bash寻找 .bash_login，如果发现.bash_login 则bash执行它。如果 .bash_login 不存在主目录中，则bash寻找 .profile 并执行它。

.profile 可以保持与 .bash_profile 或 .bash_login 的配置。它控制着出现的提示，键盘声音，要打开的 shell 以及覆盖 /etc/profile文件中设置的变量的单个配置文件设置。

### 区别
在每次交互登录时，bash shell都会执行 .bash_profile。

如果在主目录中找不到 .bash_profile，bash将执行从 .bash_login 和 .profile 中找到的第一个可读文件。

但是，在每次交互式非登录shell启动时，bash都会 .bashrc。通常情况下，环境变量会被放入 .bash_profile。

由于交互式登录shell是第一个shell，因此环境设置所需的所有默认设置都被放入.bash_profile。因此，它们只设置一次而且在所有子shell中继承。

同样地，别名和函数也会被放入 .bashrc 确保每次从现有环境中启动shell时都加载这些然而，为了避免登录和非登录交互shell设置的差异。.bash_profile 调用 .bashrc。

因此，我们将看到下面的代码片段被插入.bash_profile，以便在每个交互式登录shell上 .bashrc 也在同样shell执行：
```bash
if [ -f ~/.bashrc ];
then 
    .  ~/.bashrc; 
fi 
PATH=$PATH:$HOME/bin 
export PATH
```

#### 优先级
/etc/profile --> /etc/profile.d/* -->.bash_profile --> ~/.bashrc --> /etc/bashrc