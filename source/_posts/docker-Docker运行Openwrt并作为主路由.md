---
title: Docker运行Openwrt并作为主路由
date: 2022-04
tags: [docker, openwrt]
---

# Docker运行Openwrt并作为主路由

参考
[Docker运行Openwrt并作为主路由](https://www.starx.ink/archives/docker%E8%BF%90%E8%A1%8Copenwrt%E5%B9%B6%E4%BD%9C%E4%B8%BA%E4%B8%BB%E8%B7%AF%E7%94%B1/)

### 1.安装Docker
    curl -fsSL https://get.docker.com -o get-docker.sh
    sh get-docker.sh --mirror Aliyun

### 2.利用Docker创建虚拟内网
    docker network create -d macvlan --subnet=192.168.31.0/24 --gateway=192.168.31.1 -o parent=eth0 macnet

### 3.导入Openwrt官方armvirt_rootfs
    docker import https://downloads.openwrt.org/releases/18.06.2/targets/armvirt/64/openwrt-18.06.2-armvirt-64-default-rootfs.tar.gz openwrt:18.06.2

### 4.使用刚才创建的虚拟内网并运行（启动自启）
    docker run --restart always -d --network macnet --privileged openwrt:18.06.2 /sbin/init

### 5.进入容器内部的命令行界面，修改网络信息
    docker ps
    
    #获取正在运行的容器

    docker exec -it 1fcc16550c73 sh


### 6.修改openwrt网络配置文件
    vi /etc/config/network

    config interface 'loopback'
            option ifname 'lo'
            option proto 'static'
            option ipaddr '127.0.0.1'
            option netmask '255.0.0.0'
    
    config globals 'globals'
            option ula_prefix 'fd75:806a:fa6a::/48'
    
    config interface 'lan'
            option type 'bridge'
            option ifname 'eth0'
            option proto 'static'
            option ipaddr '192.168.31.1'
            option netmask '255.255.255.0'
            option ip6assign '60'

    将 “option ipaddr” 改成你们现在的Docker内网需要的网关地址。

    # 配置虚拟内网 在后面配置？
    ip link add link eth0 name veth0 type macvlan
    ifconfig veth0 up
    iptables -t nat -I POSTROUTING -o pppoe-wan -j MASQUERADE
    exit

### 7.建立与宿主的Docker内网通信（修改/etc/network/interfaces文件）
    echo "
    #Router
    auto mac0
    iface mac0 inet static
    address 192.168.31.2
    netmask 255.255.255.0
    gayway 192.168.31.1
    dns-nameservers 192.168.31.1
    pre-up ip link add mac0 link eth0 type macvlan mode bridge
    post-down ip link del mac0 link eth0 type macvlan mode bridge
    " >> /etc/network/interfaces

    #以上命令将建立与eth0的桥接并在Docker内网中使用192.168.31.2
    
    vi /etc/network/interfaces
    #更改eth0配置
    (iface eth0 inet dhcp)->(iface eth0 inet manual)
    
    vi /etc/NetworkManager/NetworkManager.conf
    #在以下键值内添加eth0
    #[keyfile]
    #unmanaged-devices=interface-name:wlan0
    #->
    #[keyfile]
    #unmanaged-devices=interface-name:wlan0;interface-name:eth0

### 8.测试内网通信ping

### 9.配置
#### 打开Openrt的管理页面
    使用浏览器打开 http://192.168.31.1
    进入 https://192.168.31.1/cgi-bin/luci/admin/system/startup

#### 配置PPPOE拨号上网
    浏览器打开 https://192.168.31.1/cgi-bin/luci/admin/network/network
    使用刚才Docker内网的桥接口进行配置

#### 点击添加新接口并依次输入或选择以下字符
    wan
    PPPOE
    veth0

#### 返回接口界面
    浏览器打开 https://192.168.31.1/cgi-bin/luci/admin/network/network

#### 编辑wan的参数，使用PPPOE拨号。（自行配置）

#### 配置NAT转发
    浏览器打开 https://192.168.31.1/cgi-bin/luci/admin/network/firewall/custom

    #替换为以下
    iptables -t nat -I POSTROUTING -o pppoe-wan -j MASQUERADE


#### 若光猫自带DHCP服务器，请关闭。（重要）

#### 启用强制DHCP分配
    浏览器打开 https://192.168.31.1/cgi-bin/luci/admin/network/network/lan
    DHCP服务器配置-高级设置-强制


#### SSH到Openwrt的宿主上
    #替换/etc/rc.local内容
        echo "
        modprobe pppoe
        route add default gw 192.168.31.1
        exit 0
        " > /etc/rc.local

    reboot  
