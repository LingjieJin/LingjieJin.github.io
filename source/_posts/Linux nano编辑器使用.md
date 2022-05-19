---
title: Linux Nano使用
date: 2022-05
tags: [linux, nano]
---

# Linux Nano使用

## 快捷键使用
#### 复制一整行
    Alt+6

#### 剪贴一整行
    Ctrl+K

#### 粘贴
    Ctrl+U

    如果需要复制／剪贴多行或者一行中的一部分，先将光标移动到需要复制／剪贴的文本的开头，按Ctrl+6（或者Alt+A）做标记，然后移动光标到 待复制／剪贴的文本末尾。这时选定的文本会反白，用Alt+6来复制，Ctrl+K来剪贴。若在选择文本过程中要取消，只需要再按一次Ctrl+6。

#### 搜索
    按Ctrl+W，然后输入你要搜索的关键字，回车确定。这将会定位到第一个匹配的文本，接着可以用Alt+W来定位到下一个匹配的文本。

#### 翻页
    用Ctrl+Y到上一页，Ctrl+V到下一页

#### 保存
    使用Ctrl+O来保存所做的修改

#### 退出
    按Ctrl+X

    如果你修改了文件，下面会询问你是否需要保存修改。输入Y确认保存，输入N不保存，按Ctrl+C取消返回。

    如果输入了Y，下一步会让你输入想要保存的文件名。如果不需要修改文件名直接回车就行；若想要保存成别的名字（也就是另存为）则输入新名称然后确 定。这个时候也可用Ctrl+C来取消返回。

#### 显示帮助文本
    Ctrl+G

#### 保存当前文件
    Ctrl+O

#### 读取其他文件并插入光标位置
    Ctrl+R

#### 跳至上一屏幕
    Ctrl+Y

#### 剪切当前一行
    Ctrl+K

#### 显示光标位置
    Ctrl+C

#### 退出编辑文本
    Ctrl+X

#### 对其当前段落（以空格为分隔符）
    Ctrl+J

#### 搜索文本位置
    Ctrl+W

#### 跳至下一屏幕
    Ctrl+V

#### 粘贴文本至光标处
    Ctrl+U

#### 运行拼写检查
    Ctrl+T

#### 跳转到某一行
    Ctrl+_

#### 撤销
    ALT+U

#### 重做
    ALT+E

#### 语法高亮
    ALT+Y,

#### 显示行号
    ALT+#

---
### nano配置文件
nano 配置文件(~/.nanorc)
下面配置因人而异，可以选择性添加，不过一般向类似：制表符宽度，隐藏帮助，显示行号，语法高亮，以及平滑卷屏等基本上是必开的选项。
```bash
set tabsize 4       # 设置制表符宽度
set autoindent      # 允许自动缩进
set cut             # 设置 CTRL-K 可以剪贴到行末
set noconvert       # 不要转换 DOS/UNIX 换行符
set nowrap          # 不要自动换行
set nohelp          # 不显示下面两行帮助
set morespace       # 隐藏标题下的空白行，换取更多编辑空间
set smooth          # 平滑卷屏
set suspend         # 允许 ctrl-z 将 nano 置于后台
set smarthome       # 第一次 Home 跳到行首非空字符，第二次到行首
set tabstospaces    # 展开制表符为空格（如果需要的话）
set mouse           # 允许鼠标
set linenumbers     # 显示行号（可以在编辑时 ALT-# 切换）
set backupdir path  # 设置备份路径
set backup          # 允许保存备份
set casesensitive   # 搜索使用大小写敏感
set multibuffer     # 使用 CTRL-r 读取文件时，默认读取到新缓存
set nonewlines      # 不在文件末尾添加新行
include <filename>  # 加载额外配置，通常是 /usr/share/nano 下的各种语法文件
```
---
### 获得帮助
进入nano界面后，下面有两行菜单，例如，“^G Get Help”。

其意义如下：

^G意味着快捷键是Ctrl+G，“Get Help”当然是功能了。

“^”表示Ctrl键，则Ctrl+G就表示成“^G”。

“M”表示 Alt键，则Alt+W表示为“M-W”。