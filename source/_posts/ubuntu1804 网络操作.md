# Ubuntu 网络配置有关

## 基础配置
### 修改主机名
   
    hostnamectl set-hostname ubuntu1804

### 查看ip和gateway(网关)
    
    ip addr
    route -n

### 查看DNS
    
    ls -l /etc/resolv.conf

### 查看主机名、IP地址、域名、DNS资源记录、服务的状态
    
    systemd-resolve --status

### 网卡名
默认ubuntu的网卡名称和Cent OS 7类似，如：ens33，ens38等
   1. 修改配置文件 vi /etc/default/grub
   2. 在GRUB_CMDLINE_LINUX添加内容
    GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
   3. 生效新的grub.cfg文件
    grub-mkconfig -o /boot/grub/grub.cfg
   4. 修改网卡配置文件的名称
把原来的"ensxx" 改为 “ethxx”
   5. 重启系统生效

### Ubuntu的网络配置
详情可查看[官方文档](https://help.ubuntu.com/lts/serverguide/network-configuration.html.zh-CN)
注意：配置文件中的间隔很严格，一般是两个字节，错误则网络不通
    
1. 配置动态IP地址:
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

2. 配置静态IP地址：（示例1）
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

3. 配置静态IP地址：（示例2）
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

---
## 修改IP地址、子网掩码、网关
修改网络的配置(或叫脚本)文件：
“/etc/sysconfig/network-scripts/ifcfg-ethXX”

格式如下(等号左边必须大写，因为是变量)：
DEVICE=ethXX    \\网卡名、设备名
NAME=XX         \\网络名称
BOOTPROTO=static(static和none都表示静态分配)或者写dhcp动态分配
    \\如果是动态分配，就算下面设置了IP、子网掩码、网关、DNS都不起作用)
IPADDR=IP地址
NETMASK=子网掩码，示例：255.255.255.0
PREFIX=24也是子网掩码的表示方法，“24”表示“255.255.255.0”
ONBOOT=yes      \\开机网卡是否自启。“yes”表示同意开机自启，“no”表示不同意
GATEWAY=网关      \\查看网关用“route -n”
DNS1=DNS地址      \\DNS可以把网址转换成IP，除了配置自己公司的DNS地址，还可以添加互联网上的DNS
DNS2=114.114.114.114    \\电信的DNS服务器
DNS3=8.8.8.8        \\谷歌的DNS服务器地址
DNS4=1.1.1.1        \\号称世界最快的DNS服务器，存放在澳大利亚，我们因为被“墙”阻挡，所以体验不到
DNS5=223.6.6.6或223.5.5.5       \\都是阿里DNS
DNS6=119.29.29.29               \\腾讯DNS
DNS7=180.76.76.76               \\百度DNS
查看生效的NDS“cat /etc/resolv.conf”
查看生效的网关“route -n”
在上面修改文件后，要想使文件生效，需要重启网络服务即可。
可在CentOS6，“NetworkManager服务(产线上一般都会停止这个服务)”会和“network网络服务”冲突，导致无法使配置文件生效。要生效有两种方法：

## 重启“NetworkManager服务”
service NetworkManager restart

## 关闭“NetworkManager服务”，重启“network网络服务”
Centos6: service NetworkManager status  查看服务状态
    chkconfig NetworkManager off    \\关闭自动启动
    service NetworkManager stop     \\关闭服务
    service network restart         \\重启网络服务
    注释：“NetworkManager服务停止后，在Centos6图形化界面，网络图标会消失。
Centos7: systemctl status NetworkManager 查看服务状态
    systemctl stop NetworkManager   临时停止
    systemctl disable NetworkManager 下次开机即停止服务

---
## ifconfig命令

ifconfig命令:（立即生效,重启后失效）

```
ifconfig [interface（接口）]    \\查看某某接口
ifconfig -a                     \\查看所有接口（包含未生效的接口）
ifconfig 接口 [up|down]         \\开启或禁用接口
ifconfig 接口 IP/netmask [up]   \\配置接口的IP地址网关并开启接口
ifconifg 接口  IP netmask NETMASK   \\配置接口的IP和子网掩码
```

---
## route 路由管理命令

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

---
## netstat命令

格式

    netstat [--tcp|-t] [--udp|-u] [--raw|-w] [--listening|-l] [--all|-a] [--numeric|-n] [--extend|-e[--extend|-e]] [--program|-p]
    
    -t: tcp协议相关
    -u: udp协议相关
    -w: raw socket相关
    -l: 处于监听状态
    -a: 所有状态
    -n: 以数字显示IP和端口
    -e：扩展格式
    -p: 显示相关进程及PID
    -r: 显示内核路由表

1. 示例

    常用组合：-tan, -uan, -tnl, -unl

    显示路由表：netstat {--route|-r} [--numeric|-n]

    显示接口统计数据：

    netstat {--interfaces|-I|-i} [iface] [--all|-a] [--extend|-e] [--program|-p][--numeric|-n]

2. 示例
- 当前网络的连接情况(已连接的)              netstat -nt
- 当前网络的连接情况(连接和没连接的网络)     netstat -ntua
- 监听当前程序使用的端口                    netstat -nlt
- 查看某端口被什么程序使用                  netstat -nltp
    
    示例：查看端口6000被什么程序使用(面试题)，有两种方法：
        方法一(推荐)：lsof -i :6000
        方法二：netstat -nltp :6000

- 详细查看某端口被什么程序使用              netstat -nltpe
- 网卡的连接情况 : netstat  -i
    
    Iface(网卡名)、MTU(MTU的大小)、RX-OK(接收的数据报文)、TX-OK(发送的数据报文)
    通过“watch -n 0.5 netstat -i”可观察网卡在0.5秒内使用的情况
    “watch -n 0.5 netstat -IechX”可观察网卡echX在0.5秒内使用的情况

---
## ss命令
ss命令和netstat实现的功能相似，而且netstat的选项ss都可以用。

ss还有很多netstat没有的功能，并且，在用户连接数很多的时候，ss命令可以快速的显示，效率更高。netstat通过遍历proc来获取socket信息;ss使用netlink与内核tcp_diag模块通信获取socket信息

格式：

    ss [OPTION选项]... [FILTER过滤]

    [OPTION选项]：
       * -t: tcp协议相关
       * -u: udp协议相关
       * -w: 裸套接字相关
       * -x：unix sock相关
       * -l: listen状态的连接
       * -a: 所有
       * -n: 数字格式
       * -p: 相关的程序及PID
       * -e: 扩展的信息
       * -m：内存用量
       * -o：计时器信息

    常用组合：-tan, -tanl, -tanlp, -uan

    FILTER : [ state TCP-STATE ] [ EXPRESSION ]

    1. [ state TCP-STATE ]:TCP的各种状态表达式
        TCP的常见状态：
            tcp finite state machine(有限状态机共11种):

                - LISTEN: 监听
                - ESTABLISHED：已建立的连接
                - FIN_WAIT_1
                - FIN_WAIT_2
                - SYN_SENT
                - SYN_RECV
                - CLOSED
    
    2. [ EXPRESSION ]
    
        - dport(目标端口) =
        - sport(源端口) =
        - 示例：'(dport = :ssh or sport = :ssh)'目标端口或者源端口是ssh

---
## ip 命令

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

---
## nmcli命令

nmcli是网络配置工具，功能丰富，优点是可让网卡关联多套配置。
如在公司会议室需要使用网络名office-eth1的网络配置，获取类型是静态获取IP地址；在办公室使用网络名为bangong-eth1的网络配置，获取类型是动态DHCP获取。

格式：
    
    nmcli [OPTIONS(选项)] OBJECT(过滤) { COMMAND(命令) | help }

### 常用选项
   
1. device：显示和管理网络接口, 选项具体使用可看帮助 
        
            nmcli device help
    
2. connection：启动，停止和管理网络连接, 选项具体使用可看帮助 
        
            nmcli device help

    * 修改IP地址等属性：
    
            nmcli connection modify IFACE(网卡设备的网络名) [+|-]setting(设置).property(属性) value(值)
    
            “setting.property”有很多，Tab键可补全，常用的有：
            - ipv4.addresses
            - ipv4.gateway
            - ipv4.dns1
            - ipv4.method manual | auto

    * 执行“nmcli connection modify”和修改"ifcfg-*"文件的对应关系：
    
        |   nmcli con mod                           |   ifcfg-*文件 |
        |   ---                                     |   --- |
        |   ipv4.method manual                      |   BOOTPROTO=none  |
        |   ipv4.method auto                        |   BOOTPROTO=dhcp  |
        |   ipv4.addresses“192.0.2.1/24192.0.2.254" |   IPADDR0=192.0.2.1   |
        |   PREFIXO=24                              |   GATEWAY0=192.0.2.254    |
        |   ipv4.dns8.8.8.8                         |   DNS0=8.8.8.8    |
        |   ipv4.dns-search example.com             |   DOMAIN=example.com  |
        |   ipv4.ignore-auto-dns true               |   PEERDNS=no  |
        |   connection.autoconnect yes              |   ONBOOT=yes  |
        |   connection.id etho                      |   NAME=eth0   |
        |   connection.interface-name etho          |   DEVICE=eth0 |
        |   802-3-ethernet.mac-address...           |   HWADDR=...  |

    * 生效命令
        
      * 修改网络配置文件，生效的方式
        
        方法一：
                
            systemctl restart network   \\重启网络服务
            
        方法二：
        
            nmcli con reload
    
      * 使用nmcli命令后，生效的办法
            
        方法一：
        
            nmcli con down eth0 ;nmcli con up eth0
            
        方法二：
        
            nmcli con reload

### 显示

1. 显示网络状态 
   
       nmcli connection
 
2. 显示网络设备状态
       
       nmcli device status

3. 显示网络连接配置及借款属性
 
       nmcli connection show 网络名
 
4. 显示所有包括不活动连接
 
       nmcli con show
 
5. 显示所有活动连接
 
       nmcli con show --active  

### 新建、删除、启用
 
1. 创建新连接default，IP自动通过dhcp获取
 
       nmcli con add con-name default type Ethernet ifname eth0
 
2. 删除连接
 
       nmcli con del default

3. 创建新连接static ，指定静态IP，不自动连接
        
        nmcti con add con-name static ifname eth0 autoconnect no typeEthernet ipv4.addresses 172.25.X.10/24 ipv4.gateway 172.25.X.254

4. 创建(con-name)网络名“meeting-eth1”,针对(ifname)网卡eth1,类型(type)是ethernet,开机自启(autoconnect)

        nmcli connection add con-name meeting-eth1 type ethernet autoconnect yes ifname eth1

5. 启用static连接配置

        nmcli con up static

6. 启用default连接配置

        nmcli con up default

7. 查看帮助

        nmcli con add help  

### 修改
  
1. 修改网卡设备的信息“nmcli connection modify”
    
    示例：修改网卡设备eth1的网络名为office-eth1
        
        nmcli connection modify eth1 connection.id office-eth1

2. 修改连接设置

        nmcli con mod “static” connection.autoconnect no
        
        nmcli con mod “static” ipv4.dns 172.25.X.254
        
        nmcli con mod “static” +ipv4.dns 8.8.8.8
        
        nmcli con mod “static” -ipv4.dns 8.8.8.8
        
        nmcli con mod “static” ipv4.addresses “172.16.X.10/24 172.16.X.254”
        
        nmcli con mod “static” +ipv4.addresses 10.10.10.10/16
  
3. 设置DNS

        nmcli con mod “system eth0” ipv4.ignore-auto-dns yes等价于：
        
    修改/etc/resolv.conf文件中PEERDNS=no 表示当IP通过dhcp自动获取时，dns仍是手动设置，不自动获取。DNS设置的存放在此文件。

### nmcli实现bonding

1. 添加bonding接口

    添加binding，类型是bond,名字是mybond0,接口叫bond0,模式是modeactive-backup  

        nmcli con add type bond con-name mybond0 ifname bond0 modeactive-backup  

2. 添加从属接口

    让两个网卡接口“ens3”和“ens7”加入到bonding
    
        nmcli con add type bond-slave ifname ens7 master bond0
        
        nmcli con add type bond-slave ifname ens3 master bond0  
    
    注：如无为从属接口(网卡设备)提供连接名(网络名)，则该名称是接口名称加类型构成  

3. 给bonding添加IP地址  

4. 要启动绑定，则必须首先启动从属接口

        nmcli con up bond-slave-eth0
        
        nmcli con up bond-slave-eth1  

5. 启动绑定
    
        nmcli con up mybond0  
