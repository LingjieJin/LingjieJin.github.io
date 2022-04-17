---
title: Ubuntu 18.04 配置samba
date: 2022-04
tags: [linux, ubuntu18.04, samba]
---

# Ubuntu 18.04 配置samba
参考

[How to Install and Configure Samba on Ubuntu 18.04](https://linuxize.com/post/how-to-install-and-configure-samba-on-ubuntu-18-04/)

## samba介绍

---
## 配置指令
    smbpasswd -a rhh ；增加SMB用户，同时会提示设置密码
    smbpasswd –x rhh ；删除SMB用户

    # 重启
    sudo service smbd restart

---
### 下载samba
    sudo apt-get install samba

    samba-common-3.5.10-125.el6.x86_64               //主要提供samba服务器的设置文件与设置文件语法检验程序testparm
    
    samba-client-3.5.10-125.el6.x86_64                    //客户端软件，主要提供linux主机作为客户端时，所需要的工具指令集
    
    samba-swat-3.5.10-125.el6.x86_64                    //基于https协议的samba服务器web配置界面
    
    samba-3.5.10-125.el6.x86_64                            //服务器端软件，主要提供samba服务器的守护程序，共享文档，日志的轮替，开机默认选项

---
### 增加系统用户
groupadd test -g 6000
useradd test -u 6000 -g 6000 -s /sbin/nologin -d /dev/null

---
### 修改文件权限
若不想让另人访问，只让test用户可以访问，只需执行命令:
    
    chmod u+rwx,g+rwx,o-rwx /home/test

---
### 关闭防火墙
/sbin/service iptables stop回车
/sbin/service smb restart回车
/sbin/chkconfig  --level   345   iptables   off回车

---
### 修改samba设置文件
vi /etc/samba/smb.conf ，在最后加入想要共享的文件夹：
```bash
    [rhhhome]                     ；共享名称
        path = /home/rhh      ；共享文件夹路径
        writeable = yes         ；是否可写
        guest ok = yes          ；是否允许GUEST访问
```

#### 配置root用户登录
```bash
    [alldir]
    path=/
    writable=yes
    public=yes
    valid users=root
```
设置root用户的smb服务访问密码，输入命令:
smbpasswd   –a   root，输入密码，


#### samba配置参数
```bash
[共享名]
# 说明：comment是对该共享的描述，可以是任意字符串。
comment = 任意字符串


# 说明：path用来指定共享目录的路径。可以用%u、%m这样的宏来代替路径里的unix用户和客户机的Netbios名，用宏表示主要用于[homes] 共享域。
# 例如：如果我们不打算用home段做为客户的共享，而是在/home/share/下为每个Linux用户以他的用户名建个目录，作为他的共享目录，这样path就可以写成：path = /home/share/%u; 。用户在连接到这共享时具体的路径会被他的用户名代替，要注意这个用户名路径一定要存在，否则，客户机在访问时会找不到网络路径。
#同样，如果我们不是以用户来划分目录，而是以客户机来划分目录，为网络上每台可以访问samba的机器都各自建个以它的netbios名的路径，作为不同机器的共享资源，就可以这样写：path = /home/share/%m 。
path = 共享目录路径


# 说明：browseable用来指定该共享是否可以浏览。
browseable = yes/no


# 说明：writable用来指定该共享路径是否可写。
writable = yes/no


# 说明：available用来指定该共享资源是否可用。
available = yes/no


# 说明：admin users用来指定该共享的管理员（对该共享具有完全控制权限）。在samba 3.0中，如果用户验证方式设置成“security=share”时，此项无效。
# 例如：admin users =david，sandy（多个用户中间用逗号隔开）。
admin users = 该共享的管理者


# 说明：valid users用来指定允许访问该共享资源的用户。
# 例如：valid users = david，@dave，@tech（多个用户或者组中间用逗号隔开，如果要加入一个组就用“@组名”表示。）
valid users = 允许访问该共享的用户


# 说明：invalid users用来指定不允许访问该共享资源的用户。
# 例如：invalid users = root，@bob（多个用户或者组中间用逗号隔开。）
invalid users = 禁止访问该共享的用户


# 说明：write list用来指定可以在该共享下写入文件的用户。
# 例如：write list = david，@dave
write list = 允许写入该共享的用户


# 说明：public用来指定该共享是否允许guest账户访问。
public = yes/no


# 说明：意义同“public”。
guest ok = yes/no


```

## Samba Web管理工具 SWAT 通过swat配置samba