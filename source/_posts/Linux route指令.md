---
title: Linux route 指令
date: 2022-04
tags: [linux, network, route, 指令]
---

# route 路由管理命令

1. 查看路由表：route -n
   
    路由表（重点理解）由关键的四项组成：

   - 目标网络(Destination)：目标网络的网络ID
   - 网关(Gateway)   ：到达目标网络，将数据包发送给下一个路由器的接口IP
   - 子网掩码(Genmask)    ：目标网络的网络ID的子网掩码
   - 接口(Iface)     ：从本设备的哪个接口出去，就能到达目标网络

2. 添加路由表：route add

    格式：route add [-net|-host] target [netmask Nm] [gw Gw] [[dev]If]
        添加    某网卡或本机    目标    子网掩码    网关   设备网卡名

- 示例1：目标：192.168.1.3 网关：172.16.0.1
    - route add -host 192.168.1.3 gw 172.16.0.1 dev eth0

- 示例2：目标:：192.168.0.0 网关：172.16.0.1
    - 方法一：
    route add -net 192.168.0.0 netmask 255.255.255.0 gw 172.16.0.1 dev eth0
    
    - 方法二：
    route add -net 192.168.0.0/24 gw 172.16.0.1 dev eth0

- 配置默认路由(即default或0.0.0.0，表示任何网络)，网关：172.16.0.1
    - route add -net 0.0.0.0 netmask 0.0.0.0 gw 172.16.0.1
    - route add default gw 172.16.0.1

3. 删除路由表：route del
格式:route del [-net|-host] target [gw Gw] [netmask Nm] [[dev] If]
        删除    某网卡或本机  目标    网关   子网掩码      设备网卡名
    示例1：目标：192.168.1.3 网关：172.16.0.1
    route del -host 192.168.1.3
    示例2：目标：192.168.0.0 网关：172.16.0.1
    route del -net 192.168.0.0 netmask 255.255.255.0

4. route默认没有补全命令，需要安装“bash-completion.noarch”包
    `yum -y install bash-completion.noarch`
    安装后，需要注销，重新登录就生效。
