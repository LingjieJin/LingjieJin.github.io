---
title: Linux ip 指令
date: 2022-04
tags: [linux, network, ip, 指令]
---

# ip 命令

ip命令是很强大的网络工具，已经快要替换老牌的ifconfig，并且它能实现ifconfig很多没有的功能，所以这个命令很重要，这里只是列出ip命令常用的一>些命令，具体可以查看man帮助或help。

格式：

    ip [ 选项 ] 目标 { [子命令：link | addr | route]  help(子命令帮助) }
    
    “[]”表示可选项，可写可不写；“{}”是必须项，跟着命令写的。

### 配置网络设备“ip link”，可查看数据链路层的信息
   
1. 激活或禁用指定接口“up或down”
   
        ip link set ethXX up    \\激活ethXX
        ip link set ethXX down  \\禁用ethXX
    
2. 激活或禁用指定接口““ifup或ifdown”
   
        ifup ethXX              \\激活ethXX
        ifdown ethXX            \\禁用ethXX
        
    说明：
          
    * ifup与ifdown两个程序其实是script(脚本)而已，它会直接到 /etc/ sysconfig/network-scripts目录下搜索对应的配置文件，例如ifup eth0，它会找出ifcfg-eth0这个文件的内容，然后加以设置
        
    * 这两个程序主要是搜索设置文件（ifcfg-ethx）来进行启动与关闭的，所以在使用前请确定ifcfg-ethx是否真的存在于正确的目录内，否则会启动失败。
        
    * 如果以ifconfig eth0来设置或者是修改了网络接口后，就无法再以ifdown eth0的方式来关闭了。因为ifdown会分析比较目前的网络参数与ifcfg-eth0是否相符，不符的话，就会放弃这次操作。因此，使用ifconfig修改完毕后，应该要以ifconfig eth0 down才能够关闭该接口。
    
3. 查看接口状态
   
        show ethXX      \\查看接口状态
        show up ethXX   \\查看激活状态的接口
    
4. 查看数据链路层信息“ip link”
   
        如果电脑哪天网络不通，可使用“ip link”查看网卡是哪种状态。
        “UP”状态：网线没问题
        “DOWN”状态：网线有问题

5. 配置网络设备“ip addr”，可查看或设置网络层地址
    - 添加或删除
        
        ip addr { add | del } IFADDR dev STRING
        
        * 给网卡添加IP地址时指明网卡别名[label LABEL]
        ip addr { add | del } IFADDR dev STRING label 网卡别名
        示例：ip addr add 172.16.100.100/16 dev eth0 label eth0:0
            使用命令的方式设置别名，重启服务就没了，若要永久生效，需要写配置文件vi /etc/sysconfig/network-scripts/ifcfg-eth0:0。
        注意设置别名时必须是静态ip，不能是自动获取。
        如果不想要这个别名，直接删除该文件，然后重启“network”服务即可
        
        * 指明作用域：[scope {global|link|host}]
        global: 全局可用，即两个接口进来的数据我都可以响应。是默认状态。
        link: 仅链接可用，进来的数据只有直接相连的那个接口能够响应
        host: 本机可用，即只能自己访问
        
        * 指明广播地址:[broadcast ADDRESS]
    
    - ip address show - look at protocol addresses
        [dev DEVICE]
        [label PATTERN]
        [primary and secondary]
    
    - ip addr flush 使用格式同show
        ip addr add 172.16.100.100/16 dev eth0 label eth0:0
        ip addr del 172.16.100.100/16 dev eth0 label eth0:0
        ip addr flush dev eth0 label eth0:0

6. 配置网络设备“ip route”（路由表管理）
    
    - 添加路由：ip route add
        ip route add TARGET via GW dev IFACE src SOURCE_IP
        
        TARGET:
        - 主机路由：IP
        - 网络路由：NETWORK/MASK

        示例1：ip route add 192.168.0.0/24 via 172.16.0.1

        示例2：ip route add 192.168.1.13 via 172.16.0.1
    
    - 添加网关：ip route add default via GW dev IFACE

            ip route add default via 172.16.0.1
    
    - 删除路由：

            ip route del TARGET
    
    - 显示路由：

            ip route show|list
    
    - 清空路由表
            
            ip route flush [dev IFACE] [via PREFIX]
            ip route flush dev eth0
            ip route flush(全清空)

    ```
    说明：
    使用命令的方式增加或者删除路由记录，都是临时的，如果重启network服务，那么操作就失效了。想要永久生效可以编辑配置文件/etc/sysconfig/network-scripts/route-eth*，步骤如下：
    1、 vim etc/sysconfig/network-scripts/route-eth0
        文件内容有两种写法：
            1）单行
                netid/mask via gw  比如2.2.2.2/16 via 10.0.0.0
            2）多行
                ADDRESS#=目标网络
                NETMASK#=子网掩码
                GATEWAY#=网关
    注意：
    同一路由记录的#数字必须一样，因为可能会添加多条路由，数字一样的为同一组。
    同一个文件里，两种格式不能混合着写。
          2、重启服务
                 Centos6:  service network restart
                 Centos7:  systemctl restart network 
    ```
