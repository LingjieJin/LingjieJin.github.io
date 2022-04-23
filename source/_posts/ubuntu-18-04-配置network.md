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
        dns-nameservers 8.8.8.8

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