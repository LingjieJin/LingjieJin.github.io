---
title: Linux systemctl 指令
date: 2022-04
tags: [linux, system, systemctl, 指令]
---

# Linux systemctl 指令
systemctl mask和systemctl disable的区别一般很难注意到，因为我大部分时候只会使用systemctl disable，并不会用到systemctl mask。在一次遇到问题的时候，需要使用systemctl mask来禁用服务，下边具体说明。

---
## systemctl enable的作用
我们知道，在系统中安装了某个服务以后，需要将该服务设置为开机自启，那么一般会执行systemctl enable xxx，这个时候会发现shell中会输出两行提示，一般类似如下：

    # systemctl enable NetworkManager 
    
    Created symlink from /etc/systemd/system/multi-user.target.wants/NetworkManager.service to /usr/lib/systemd/system/NetworkManager.service.
    
    Created symlink from /etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service to /usr/lib/systemd/system/NetworkManager-dispatcher.service.
    
    Created symlink from /etc/systemd/system/network-online.target.wants/NetworkManager-wait-online.service to /usr/lib/systemd/system/NetworkManager-wait-online.service.

这个命令会在/etc/systemd/system/目录下创建需要的符号链接，表示服务需要进行启动。

通过stdout输出的信息可以看到，软连接实际指向的文件为/usr/lib/systemd/system/目录中的文件，实际起作用的也是这个目录中的文件。

---
## systemctl disable xxx的作用
执行systemctl disable xxx后，会禁用这个服务。它实现的方法是将服务对应的软连接从/etc/systemd/system中删除。命令执行情况一般类似如下：
    
    systemctl disable NetworkManager
    
    Removed symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service.
    
    Removed symlink /etc/systemd/system/dbus-org.freedesktop.NetworkManager.service.
    
    Removed symlink /etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service.
    
    Removed symlink /etc/systemd/system/network-online.target.wants/NetworkManager-wait-online.service.

在执行systemctl disable xxx的时候，实际只是删除了软连接，并不会产生其他影响。

---
## systemctl mask xxx的作用
执行 systemctl mask xxx会屏蔽这个服务。它和systemctl disable xxx的区别在于，前者只是删除了符号链接，后者会建立一个指向/dev/null的符号链接，这样，即使有其他服务要启动被mask的服务，仍然无法执行成功。执行该命令的效果一般类似如下：

    systemctl mask NetworkManager 
    
    Created symlink from /etc/systemd/system/NetworkManager.service to /dev/null.

---
### systemctl mask xxx和systemctl disable xxx的区别
在执行过mask后，如果想要启动服务，那么会报类似如下错误：

    systemctl start NetworkManager
    Failed to start NetworkManager.service: Unit is masked.

如果使用disable的话，可以正常启动服务。总体来看，disable和enable是一对操作，是用来启动、停止服务。

---
## systemctl unmask xxx取消屏蔽
如果使用了mask，要想重新启动服务，必须先执行unmask将服务取消屏蔽。mask和unmask是一对操作，用来屏蔽和取消屏蔽服务。
