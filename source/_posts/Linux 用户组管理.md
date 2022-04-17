---
title: Linux 用户组管理
date: 2022-04
tags: [linux, group]
---

# Linux 用户组管理

---
### groupadd
groupadd 命令使用命令行上指定的值以及系统中的默认值创建一个新的组帐户。新组将根据需要被添加到系统文件中。

groupadd 命令 语法格式如下：
    
    groupadd [-g gid [-o]] [-r] [-f] group
    参数说明:
    -g：指定新建工作组的 id；
    -r：创建系统工作组，系统工作组的组ID小于 500；
    -K：覆盖配置文件 "/ect/login.defs"；
    -o：允许添加组 ID 号不唯一的工作组。
    -f,--force: 如果指定的组已经存在，此选项将失明了仅以成功状态退出。当与 -g 一起使用，并且指定的GID_MIN已经存在时，选择另一个唯一的GID（即-g关闭）。

---
### groupmod
groupmod 命令用于修改用户组的相关信息，命令格式如下：

    groupmod [选现] 组名
    选项：
    -g GID：修改组 ID；
    -n 新组名：修改组名；

---
### groupdel
    groupdel [群组名称]

