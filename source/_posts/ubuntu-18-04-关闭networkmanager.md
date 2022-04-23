---
title: Ubuntu 18.04 关闭networkmanager
date: 2022-04
tags: [linux, ubuntu18.04, networkmanager]
---

# Ubuntu 18.04 关闭networkmanager

## 参考

[Ubuntu中启用关闭Network-manager网络设置问题](https://blog.csdn.net/anhuidelinger/article/details/17584299)

这篇介绍了ubuntu 18.04如何将设置修改为interface
[Ubuntu 18.04: switch back to /etc/network/interfaces](https://askubuntu.com/questions/1031709/ubuntu-18-04-switch-back-to-etc-network-interfaces)

Ubuntu 官方文档
[Ubuntu-NetworkManager](https://help.ubuntu.com/community/NetworkManager#Stopping_and_Disabling_NetworkManager)

[How to switch from Network Manager to systemd-networkd on Linux](https://www.xmodulo.com/switch-from-networkmanager-to-systemd-networkd.html)

## 关闭NetworkManager
### 1. 重新安装ifupdown package

    # apt-get update
    # apt-get install ifupdown

    删除netplan
    $ sudo apt remove netplan
    $ rm -rf /etc/netplan

    删除network-manager
    sudo apt-get purge network-manager

### 2.修改interface配置
    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    allow-hotplug enp0s3
    auto enp0s3
    iface enp0s3 inet static
    address 192.168.1.133
    netmask 255.255.255.0
    broadcast 192.168.1.255
    gateway 192.168.1.1
    # Only relevant if you make use of RESOLVCONF(8)
    # or similar...
    dns-nameservers 1.1.1.1 1.0.0.1

### 3.使能配置（不需要重启）
    # ifdown --force enp0s3 lo && ifup -a
    # systemctl unmask networking
    # systemctl enable networking
    # systemctl restart networking

### 4. 关闭NetworkManager
The steps to disable NetworkManager depend on which of the initialization subsystems are running: Upstart or Systemd.

#### Using Upstart
According to this bug here's how to disable Network Manager without uninstalling it:

1. Stop network manager
    sudo stop network-manager

2. Create an override file for the Upstart job:
    echo "manual" | sudo tee /etc/init/network-manager.override


#### Using Systemd
Systemd became the default initialization system in Ubuntu 15.04. Here's how to stop and disable Network Manager without uninstalling it (taken from AskUbuntu):

1. Stop network manager

    sudo systemctl stop NetworkManager.service
    sudo systemctl stop NetworkManager-wait-online.service
    sudo systemctl stop NetworkManager-dispatcher.service
    sudo systemctl stop network-manager.service

2. Disable network manager (permanently) to avoid it restarting after a reboot

    sudo systemctl disable NetworkManager.service
    sudo systemctl disable NetworkManager-wait-online.service
    sudo systemctl disable NetworkManager-dispatcher.service
    sudo systemctl disable network-manager.service

或者

    # systemctl stop systemd-networkd.socket systemd-networkd \
    networkd-dispatcher systemd-networkd-wait-online
    # systemctl disable systemd-networkd.socket systemd-networkd \
    networkd-dispatcher systemd-networkd-wait-online
    # systemctl mask systemd-networkd.socket systemd-networkd \
    networkd-dispatcher systemd-networkd-wait-online
    # apt-get --assume-yes purge nplan netplan.io

### 5. 配置DNS
Because Ubuntu Bionic Beaver (18.04) make use of the DNS stub resolver as provided by SYSTEMD-RESOLVED.SERVICE(8), you SHOULD also add the DNS to contact into the /etc/systemd/resolved.conf file. For instance:

    ....
    DNS=1.1.1.1 1.0.0.1
    ....
and then restart the systemd-resolved service once done:

    # systemctl restart systemd-resolved

This will make sure that the resolver can be updated by the DHCP-client, like it was before when using interfaces.

    rm /etc/resolv.conf
    ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf

### 6. 启动Systemd
接下来，启动并启用systemd-networkd：

    sudo systemctl unmask systemd-networkd.service
    sudo systemctl enable systemd-networkd.service
    sudo systemctl start systemd-networkd.service


---
## 启用NetworkManager并禁用systemd-networkd
可以通过以下步骤启动和启用Ubuntu Network Manager（在Ubuntu服务器中不建议这样做）。

1. 首先，停止系统联网服务：

        sudo systemctl disable systemd-networkd.service
        sudo systemctl mask systemd-networkd.service
        sudo systemctl stop systemd-networkd.service

2. 在Ubuntu上安装NetworkManager：
    
        sudo apt-get install network-manager

3. 打开/etc/netplan目录中的.yaml配置文件，并用以下内容替换现有配置：
        
        network:
            version: 2
            renderer: NetworkManager

4. 使用netplan命令为NetworkManager生成特定于后端的配置文件：
    
        sudo netplan generate

5. 启动NetworkManager服务：

        sudo systemctl unmask NetworkManager
        sudo systemctl enable NetworkManager
        sudo systemctl start NetworkManager

6. 现在启用了NetworkManager，可以使用nmcli命令通过GUI或从命令行完成接口配置


---
## networking启动耗时问题
systemd-analyze blame输出 :

 sudo nano /lib/systemd/system/networking.service