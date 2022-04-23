---
title: Ubuntu 18.04 关闭networkmanager
date: 2022-04
tags: [linux, ubuntu18.04, networkmanager]
---

# Ubuntu 18.04 关闭networkmanager
参考

[Ubuntu中启用关闭Network-manager网络设置问题](https://blog.csdn.net/anhuidelinger/article/details/17584299)


#### 禁用网络管理器并启用systemd-networkd
首先，运行以下命令以禁用NetworkManager：

    sudo systemctl stop NetworkManager
    sudo systemctl disable NetworkManager
    sudo systemctl mask NetworkManager

接下来，启动并启用systemd-networkd：

    sudo systemctl unmask systemd-networkd.service
    sudo systemctl enable systemd-networkd.service
    sudo systemctl start systemd-networkd.service


#### 启用NetworkManager并禁用systemd-networkd
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


### Debian/Ubuntu的Network-Manager

debian/ubuntu管理网络连接的有两个东西，图形化的NetworkManager和文字的 ifup/ifdown，如果在 /etc/network/interfaces里设置了网卡信息，那么NetworkManager就不会接管该网卡，如果没有设置NetworkManager默认是会接管网卡的。

NetworkManager最方便的地方是个人电脑用无线连网的时候，因为文字界面的 iwlist wlan0 scan 并不是非常好用，而NetworkManger可以像windows那样列出所有可用的wifi热点，如果有中文乱码的，也一样可以连接，但如果你的机子是用来做服务器的，或者是在办公室里使用，有固定的路由环境，一般来说，也会有固定的ip，因为这样可以方便内网共享资源。总之不是个人用的，而且不用移来移去的话，最好是关掉这个NetworkManager，当然如果你经常带着本子跑，想去咖啡馆蹭网的话，就一定要打开这个NetworkManager，自动搜索无线非常方便了。
    
下面来说说这两种情况：

1. 使用NetworkManger来联网，这个时候，如果使用了无线连接路由，而在/etc/network/interfaces里配置了有线连接的eth0的话，就会发生一个超级诡异的问题：可以访问外网，但不能访问内网，比如无线路由ip 192.168.0.1，ping该地址，会显示有线网卡的ip地址无法ping通，而不是无线！证明在设置了有线eth0的时候，会优先采用，但奇怪的是，外网连接正常，所有我怀疑这可能是debian的一个bug。所以当使用 NetworkManager的时候，可以注释掉所有/etc/network/interfaces 里的内容，仅仅保留本地回环网络：
        auto lo
        iface lo inet loopback
    这两句。设置固定ip，可以在NetworkManager图形界面里配置。
    
2. 关闭NetworkManager

关闭命令：
    
    sudo /etc/init.d/network-manager stop 

取消开机启动：
    
    chkconfig network-manager off 

重启网络：
    
    /etc/init.d/networking restart
    
修改 /etc/network/interfaces 文件

    系统配置部分：本地回环网络。
        auto lo
        iface lo inet loopback
    
    有线配置部分：
        auto eth0
        #iface eth0 inet dhcp # 如果你不想用固定ip的话，推荐用固定ip，这样可以省去请求路由分配的时间
        iface eth0 inet static
        netmask 255.255.255.0
        gateway 192.168.0.1      #gateway 0.0.0.0 # 拨号上网请把 gateway全部设置为0
        address 192.168.0.112
    
    无线配置部分：
        auto wlan0
        iface wlan0 inet static
        netmask 255.255.255.0
        gateway 192.168.0.1
        address 192.168.0.113
        pre-up ip link set wlan0 up
        pre-up iwconfig wlan0 essid ssid
        wpa-ssid TP-Link # 这里的ssid为路由里设置的无线名称
        wpa-psk 12345678 # 无线密码
    
    adsl拨号上网：
        auto dsl-provider
        iface dsl-provider inet ppp # dsl-provider 为之前配置好的拨号名称
        provider dsl-provider