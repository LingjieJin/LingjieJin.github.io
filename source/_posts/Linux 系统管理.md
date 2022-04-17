---
title: Linux 系统管理
date: 2022-04
tags: [linux]
---

# Linux 系统管理
1. 查看版本号 cat /etc/issue
2. uname -a
3. cat /prov/version

# 查看ip和gateway(网关)
    
    ip addr
    route -n

# 查看DNS
    
    ls -l /etc/resolv.conf


# 固定主机名
编辑两个文件
1. /etc/hostname
2. /etc/hosts 修改其中的一个本机名字

# 设置ssh登录
1. 安装ssh: apt install openssh-server
2. 查看服务是否开启: ps -aux | grep ''ssh'' 如果没sshd服务可以手动开启sudo service ssh start

# linux查看网卡信息
使用ifconfig命令查看网卡名称
使用ethtool命令查看网卡是否为千兆网卡