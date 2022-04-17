---
title: Linux nmcli 指令
date: 2022-04
tags: [linux, network, nmcli, 指令]
---

# nmcli命令

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
