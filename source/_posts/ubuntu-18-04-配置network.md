---
title: Ubuntu 18.04 配置network的interface
date: 2022-04
tags: [linux, ubuntu18.04, network interface]
---

# Ubuntu 18.04 配置network的interface

#### 安装软件
    sudo apt install ifupdown

#### 修改配置

    sudo nano /etc/network/interfaces


    auto enp2s0
    iface enp2s0 inet static
        address 192.168.1.197
        netmask 255.255.255.0
        gateway 192.168.1.1
        network 192.168.1.0
        broadcast 192.168.1.255 
        dns-nameservers 8.8.8.8

    auto enp2s0
    iface enp2s0 inet dhcp

    auto enp0s3 — automatic launch of a specific interface;
    iface enp0s3 inet static — reports on static configuration;
    address 10.10.2.6 —actually, the IPv4 address for this interface;
    netmask 255.255.255.0 — netmask.
    gateway 10.10.2.1 — IPv4-gateway
    dns-nameservers 8.8.8.8 — specify DNS servers

#### 修改 DNS
打开/etc/resolv.conf

    searchlocaldomain #如果本Server为DNS服务器，可以加上这一句，如果不是，可以不加

    nameserver 172.16.3.4 #希望修改成的DNS

    nameserver 172.16.3.3 #希望修改成的DNS

#### 重启配置
    sudo /etc/init.d/networking restart

#### 修改设置
打开命令行，输入以下代码
    
        sudo gedit /etc/NetworkManager/NetworkManager.conf

类似于上面的操作，打开该文件，将“managed=false” 修改为 “managed=true”。
意思是，将网络连接设置为自定义或手动。#号后面的是注释内容

修改设置

重启network manager：
        
        sudo service network-manager restart

重启系统后，发现依然可以正常使用静态ip。

#### 修改网卡名称（默认的Ubuntu网卡名称不是eth0 wlan0）

    sudo gedit /etc/default/grub

    找到GRUB_CMDLINE_LINUX=""
    改为GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"