---
title: docker 配置openwrt 单网卡 旁路由
date: 2022-04
tags: [docker, openwrt]
---

# docker 配置openwrt 单网卡 旁路由
参考:[docker 运行 openwrt](https://github.com/luoqeng/OpenWrt-on-Docker)

### 配置docker环境

#### 创建一个名为 macvlan0 的新 macvlan 网络
    网段与网关请替换成自己所处的网络 ens34请替换成自己的接口名

    sudo docker network create \
        -d macvlan \
        --subnet=192.168.3.0/24 \
        --gateway=192.168.3.1 \
        -o parent=ens34 
         macvlan0

#### 验证是否已创建 macvlan0 网络
    sudo docker network ls

#### 检查网络详细信息
    sudo docker network inspect macvlan0

#### 运行透明代理版 openwrt
    请替换成自己所处的网段ip
        
    sudo docker run \
        --privileged \
        --name='openwrt' \
        --net=macvlan0 \
        --ip=192.168.3.254 \
        --detach=true \
        luoqeng/openwrt:18.06.2 \
        /sbin/init

    当前各种自发明加密协议基本残废，跑在 TLS 协议里面比较稳妥，推荐运行原版 openwrt
    
    导入镜像:
    sudo docker import \
    https://downloads.openwrt.org/releases/18.06.2/targets/x86/64/openwrt-18.06.2-x86-64-generic-rootfs.tar.gz \
    openwrt:18.06.2

#### 创建并启动容器
    sudo docker run -d \
        --restart unless-stopped \
        --network macvlan0 \
        --ip=192.168.3.254 \
        --privileged \
        --name openwrt \
        openwrt:18.06.2 \
        /sbin/init
    
    注意：Docker 的 IPAM 驱动程序不知道外部 DHCP 客户端已在使用的 IP 地址，从而导致子网中可能存在 IP 地址冲突。不应该让外部 DHCP 服务器与您在创建 macvlan 网络时配置的同一子网分配 IP 地址。但我们是单网卡路由忽略这一点，取后面几位 IP 尽量避免冲突 :)

#### 验证 openwrt 是否正在运行
    sudo docker ps

---
## 配置docker openwrt环境
配置 openwrt

#### 进入容器
sudo docker exec -it openwrt /bin/sh

#### 配置openwrt网络
```bash
编辑 /etc/config/network
config interface 'lan'
    option type 'bridge'
    option ifname 'eth0'
    option proto 'static'
    option ipaddr '192.168.3.254' #请替换成 docker run --ip 的参数
    option netmask '255.255.255.0'
    option ip6assign '60'
    option gateway '192.168.3.1' #请替换成 docker network create --gateway 的参数
    option dns '127.0.0.1'
```

#### 重启网络生效
    /etc/init.d/network restart

---
## 配置 docker host，使openwrt和宿主机通信
#### 创建一个名为 macvlan1 的新 macvlan 网络，让其通过 openwrt 上网
    sudo ip link add macvlan1 link ens34 type macvlan mode bridge

#### 删除老的路由
    sudo ip route del 192.168.3.0/24

#### 设置 macvlan1 ip
    sudo ip addr add 192.168.3.253/24 dev macvlan1

#### 启用 macvlan1
    sudo ip link set macvlan1 up

#### 删除以前的默认网关
    sudo ip route del default

#### 添加新的默认网关
    sudo ip route add default via 192.168.3.254 dev macvlan1

#### 测试网络
    ping www.baidu.com

#### 查看ip
    ip a

#### 查看路由
    ip route