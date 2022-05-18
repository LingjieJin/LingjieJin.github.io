vim .bashrc
添加下行
export PS1="Time:\[\033[1;35m\]\T     \[\033[0m\]User:\[\033[1;33m\]\u     \[\033[0m\]Dir:\[\033[1;32m\]\w\[\033[0m\]\n\$"
退出vim
source .bashrc

echo "export PS1="Time:\[33[1;35m\]\T \[33[0m\]User:\[33[1;33m\]\u \[33[0m\]Dir:\[33[1;32m\]\w\[33[0m\]\n\$" " >> .bashrc

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
PS1="
 🐂  $PROMPT_SECTION_SHELL 🍎  $PROMPT_SECTION_DIRECTORY 🌹  $PROMPT_SECTION_GIT_BRANCH 🌹
$PROMPT_SECTION_ARROW "


export PS1='[\[\e[01;05;32m\]\u\[\e[00m\]@\[\e[01;33m\]\h\[\e[00m\]:\[\e[01;34m\]\w\[\e[00m\]]\$ '