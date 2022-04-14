---
title: ubuntu 18.04 使用netplan设置网络
date: 2022-04-14 11:28:47
tags: ubuntu18.04 netplan  
---

# ubuntu 18.04 netpan 设置

## 1. 配置动态IP地址:
    ```
    cat /etc/netplan/01-netcfg.yaml
    network:
      version: 2
      renderer: networkd
      ethernets:
      ens33:
        dhcp4: yes
    ```
    修改网卡配置文件后需执行命令生效：netplan apply

## 2. 配置静态IP地址：（示例1）
    ```
    cat /etc/netplan/01-netcfg.yaml
    network:
    version: 2
    renderer: networkd
    ethernets:
    eth0:
        dhcp4: no
    addresses:
      - 192.168.6.10/24
      - 10.10.10.10/24(可配置多个IP地址)
      gateway4: 192.168.6.1
      nameservers:
        addresses: [223.5.5.5, 8.8.8.8, 1.1.1.1]
    ```

## 3. 配置静态IP地址：（示例2）
    ```
    cat /etc/netplan/01-netcfg.yaml
    network:
        version: 2
        renderer: networkd
        ethernets:
            eth0:
            dhcp4: no
            addresses:
              - 172.20.148.39/16
            gateway4: 172.20.0.1
            nameservers:
                addresses: [223.5.5.5,114.114.114.114]
    ```
    使网卡配置生效：netplan apply
    > 说明：
    addresses \\设置IP地址
    gateway4   \\设置网关
    nameservers  \\设置DNS

