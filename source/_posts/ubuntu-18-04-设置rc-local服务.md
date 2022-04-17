---
title: ubuntu 18.04 设置rc-local服务
date: 2022-04
tags: [linux, ubuntu18.04,system,rc-local]
---

# ubuntu 18.04 设置rc-local服务
ubuntu18.04不再使用initd管理系统，改用systemd。
然而systemd很难用，改变太大，跟之前的完全不同。

使用systemd设置开机启动
为了像以前一样，在/etc/rc.local中设置开机启动程序，需要以下几步：

### 1、配置链接
systemd默认读取/etc/systemd/system下的配置文件，该目录下的文件会链接/lib/systemd/system/下的文件。一般系统安装完/lib/systemd/system/下会有rc-local.service文件，即我们需要的配置文件。
链接过来：

        ln -fs /lib/systemd/system/rc-local.service /etc/systemd/system/rc-local.service
    
        cd /etc/systemd/system/
    
        cat rc-local.service
    
rc-local.service内容
```
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

# This unit gets pulled automatically into multi-user.target by
# systemd-rc-local-generator if /etc/rc.local is executable.
[Unit]
Description=/etc/rc.local Compatibility
Documentation=man:systemd-rc-local-generator(8)
ConditionFileIsExecutable=/etc/rc.local
After=network.target

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
RemainAfterExit=yes
GuessMainPID=no

[Install]
WantedBy=multi-user.target
Alias=rc-local.service
```
1. [Unit] 区块：启动顺序与依赖关系。 
2. [Service] 区块：启动行为,如何启动，启动类型。 
3. [Install] 区块，定义如何安装这个配置文件，即怎样做到开机启动。


### 2.创建/etc/rc.local文件
touch /etc/rc.local

### 3. 赋可执行权限
chmod 755 /etc/rc.local

### 4.编辑rc.local，添加需要开机启动的任务
    #!/bin/bash
    echo "test rc " > /var/test.log

### 5.执行reboot重启系统，然后查看test.log