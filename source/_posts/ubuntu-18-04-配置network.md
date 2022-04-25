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

### interface参数
#### auto与allow-hotplug的区别
1. auto

    auto <interface_name>
    
    含义：
    
    在系统启动的时候启动网络接口,无论网络接口有无连接(插入网线),如果该接口配置了DHCP,则无论有无网线,系统都会去执行DHCP,如果没有插入网线,则等该接口超时后才会继续。

2. allow-hotplug

    allow-hotplug <interface_name>

    含义：
    
    只有当内核从该接口检测到热插拔事件后才启动该接口。如果系统开机时该接口没有插入网线,则系统不会启动该接口,系统启动后,如果插入网线,系统会自动启动该接口。也就是将网络接口设置为热插拔模式。

#### 设置开机启动 promisc
在每个网卡驱动下加入：
up ip link set enpxxx promisc on