# Linux账户管理

## 账户管理
---
### 增加用户
Linux 中通常会使用 useradd，而 Ubuntu 中通常使用 adduser。
useradd 选项 用户名
参数说明：
选项:
-c comment 指定一段注释性描述。
-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
-g 用户组 指定用户所属的用户组。
-G 用户组，用户组 指定用户所属的附加组。
-s Shell文件 指定用户的登录Shell。
-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

只有 root 才能将用户或组添加到系统， 默认为 root。
[sudo] adduser newuser
adduser 可以不带任何参数使用，自动添加用户 newuser 到新组 newuser 、创建主目录 /home/newuser、 提示设置密码和用户信息。

[sudo] useradd newuser
[sudo] passwd newuser
[sudo] useradd -d /home/newuser newuser
useradd 创建用户分三步：用户名，密码和主目录。

---
### 删除用户
删除一个已有的用户账号使用userdel命令，其格式如下：
userdel 选项 用户名
常用的选项是 -r，它的作用是把用户的主目录一起删除。

删除
userdel username

id username 查看是否删除

---
### 修改用户信息
修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。

修改已有用户的信息使用usermod命令，其格式如下：

usermod 选项 用户名
常用的选项包括-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值。

另外，有些系统可以使用选项：-l 新用户名

### 密码管理
passwd 选项 用户名
可使用的选项：

-l 锁定口令，即禁用账号。
-u 口令解锁。
-d 使账号无口令。
-f 强迫用户下次登录时修改口令。

---
## 用户组管理

### 用户组管理
groupadd 选项 用户组
可以使用的选项有：

-g GID 指定新用户组的组标识号（GID）。
-o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。


---
## 查看用户列表
cat /etc/passwd

---
## 增加sudo root权限
为用户添加 sudo 权限，可以使用修改 sudoers 和 adduser 两种方法：
etc/sudoers 文件就是与 sudo 组有关的文件，在里面添加一行
newuser ALL=(ALL) ALL

第二种是使用 adduser 命令，添加用户的同时赋予 root 权限：
[sudo] adduser newuser sudo

