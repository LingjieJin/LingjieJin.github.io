---
title: Linux 用户管理
date: 2022-04
tags: [linux, user]
---

# Linux 用户管理

---
### 增加用户
Linux 中通常会使用 useradd，而 Ubuntu 中通常使用 adduser。
    
    useradd 选项 用户名
    参数说明：
    -c<备注> 　加上备注文字。备注文字会保存在passwd的备注栏位中。
    -d<登入目录> 　指定用户登入时的起始目录。
    -D 　变更预设值．
    -e<有效期限> 　指定帐号的有效期限。
    -f<缓冲天数> 　指定在密码过期后多少天即关闭该帐号。
    -g<群组> 　指定用户所属的群组。
    -G<群组> 　指定用户所属的附加群组。
    -m 　自动建立用户的登入目录。
    -M 　不要自动建立用户的登入目录。
    -n 　取消建立以用户名称为名的群组．
    -r 　建立系统帐号。
    -s<shell>　 　指定用户登入后所使用的shell。
    -u<uid> 　指定用户ID。

只有 root 才能将用户或组添加到系统， 默认为 root。
    
    [sudo] adduser newuser

    adduser 可以不带任何参数使用，自动添加用户 newuser 到新组 newuser 、创建主目录 /home/newuser、 提示设置密码和用户信息。

    [sudo] useradd newuser
    [sudo] passwd newuser
    [sudo] useradd -d /home/newuser newuser
    
    useradd 创建用户分三步：用户名，密码和主目录。

举例：
1. 添加一个不能登录的用户
    
    useradd -d /usr/local/apache -g apache -s /bin/false apache
    
    要拒绝系统用户登录，可以将其 shell 设置为 /usr/sbin/nologin 或者 /bin/false。

---
### 删除用户
删除一个已有的用户账号使用userdel命令，其格式如下：
    
    userdel 选项 用户名
    常用的选项是 -r，它的作用是把用户的主目录一起删除。
    要刪除已登入的使用者賬戶或與之相關的正在執行的程序，我們可以使用 userdel 命令中的 -f 或 --force 選項。

    id username 查看是否删除

---
### 修改用户信息
修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。

修改已有用户的信息使用usermod命令，其格式如下：

    usermod 选项 用户名
    选项包括-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值。
    参数：

    -a<追加> 必须与-G选项一起使用，把用户追加到某些组中。
    -c<备注> 修改用户帐号的备注文字。
    -d<登入目录> 修改用户登入时的目录。
    -e<有效期限> 修改帐号的有效期限。
    -f<缓冲天数> 修改在密码过期后多少天即关闭该帐号。
    -g<群组> 修改用户所属的群组。
    -G<群组> 修改用户所属的附加群组。
    -l<帐号名称> 修改用户帐号名称。
    -L 锁定用户密码，使密码无效。
    -s<shell> 修改用户登入后所使用的shell。
    -u<uid> 修改用户ID。
    -U 解除密码锁定。

    另外，有些系统可以使用选项：-l 新用户名

举例：
1. usermod最典型的用例是将用户添加到组中。要将现有用户添加到辅助组，请使用-a和-G选项，然后输入组的名称和用户名。

        usermod -a -G GROUP USER
            
        如果要一次将用户添加到多个组，请在-G选项后指定由逗号,分隔的组，中间不留空格。例如，要将用户myfreax添加到games组，您可以运行以下命令：
            
        sudo usermod -a -G games myfreax

2. 这是清空用户testUser1所有的追加组，执行该命令以后testUser1只属于testUser1这个默认组，不管testUser1以前有多少个附加组。

        usermod -G "" testUser1

3. 让用户testUser1的附加组变成testUser1,testGroup1,testGroup4，这个命令有点类似usermod -G "" testUser1，只不过usermod -G "" testUser1是清空，而usermod -G  testGroup2,testGroup3 testUser1 则是把 testGroup2,testGroup3以外的组清空，而保留testGroup2,testGroup3。因为gpasswd -d  一次只能删除一个，所以用这种方法可以一步直达。

        usermod -G  testGroup2,testGroup3 testUser1

4. 删除用户testUser1的testGroup2所属组，即把用户testUser1从testGroup2 组中剔除，执行以后用户testUser1属于testUser1,testGroup1,testGroup3,testGroup4组，注 意，gpasswd -d只能一个组一个组操作，如果要删除多个组则只能操作多次

        gpasswd -d  testUser1 testGroup2

5. 假设系统已经存在testGroup1,testGroup2两个用户组和一个用户testUser1，现在要把testUser1加到testGroup1,testGroup2两个用户组中

        usermod -a -G  testGroup1,testGroup2  testUser1

        或者

        usermod -a -G  testGroup1 testUser1

        usermod -a -G  testGroup2 testUser1

6. 假设系统已经存在testGroup1,testGroup2两个用户组，现在要新增一个用户testUser1，同时让testUser1属于testGroup1,testGroup2两个用户组

        useradd -G  testGroup1,testGroup2  testUser1


---
### 修改属性
#### 将文件或目录的拥有者(用户)修改为指定的用户名
    chown [选项] 用户名 文件名|目录　　　 

#### 将文件或目录的所属组修改为指定的组
    chgrp [选项] 组名 文件名|目录 　　　 

#### 修改权限的方式有两种：
    chmod [选项] .. {对象-操作-权限} 文件名|目录
    选项：-R 表示当为目录的时候递归修改。即把该目录下的所有文件和目录一并修改
    对象：a 表示所有人；u 表示拥有者；g 表示所属组 ； o 表示其他人
    操作：+ 表示增加权限 ； - 表示减少权限
    权限 ： r 读； w 写 ； x 执行用三个数字分别是拥有者、组、其他人的权限。
    777 表示 u=rwx,g=rwx,o=rwx
    755 表示 u=rwx,g=rx,o=rx
    644 表示 u=rw,g=r,o=r

举例：
1. 对所有对象减少目录a的写权限

        sudo chmod a-w 目录a 

2. 对目录a的权限设置是拥有者rwx，组rx，其他人rx
    
        sudo chmod -R 755 目录a 

---
### 密码管理
    passwd 选项 用户名
    可使用的选项：
    -l 锁定口令，即禁用账号。
    -u 口令解锁。
    -d 使账号无口令。
    -f 强迫用户下次登录时修改口令。

---
## 查看用户有关信息

    #查看用户列表
    cat /etc/passwd

    #查看群组消息
    cat /etc/group

    #查看用户的UID(用户id) 和 GID(组id) 和附件组等， 用户不写，默认当前用户
    id [选项]….[用户]..　　　 
    
    #查看目前所有登录的用户和用户IP地址和时间,未登录用户不能显示
    who [选项]… 　　　 

    #查看我是哪个用户
    whoami [选项] …..　　　 

    #查看系统负载与用户
    w 　　　 

    tty1～tty6:本地控制台终端；tty7表示图形终端；pts/0~255表示虚拟终端

---
## 增加sudo root权限
为用户添加 sudo 权限，可以使用修改 sudoers 和 adduser 两种方法：
1. etc/sudoers 文件就是与 sudo 组有关的文件，在里面添加一行
        
        newuser ALL=(ALL) ALL

2. 第二种是使用 adduser 命令，添加用户的同时赋予 root 权限：
    
        [sudo] adduser newuser sudo