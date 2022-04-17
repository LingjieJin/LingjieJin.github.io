---
title: docker 配置openwrt
date: 2022-04
tags: [docker, openwrt]
---


# 在docker中启动openwrt
参考[在docker里运行openwrt](https://blog.hearthewind.top/posts/db1132b0-4e36-11eb-bbe3-c117a7825fa4/)

---
## 安装docker
#### 安装必要软件
    sudo apt install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common

#### 添加GPG密钥
    curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/raspbian/gpg | sudo apt-key add -

#### 向sourcelist中添加Docker软件源
    sudo add-apt-repository \
        "deb [arch=armhf] https://mirrors.aliyun.com/docker-ce/linux/raspbian \
        $(lsb_release -cs) \
        stable"

#### 安装docker
    apt update	#刷新包缓存
    apt install docker-ce	#安装docker
    docker info #查看docker状态

---
## 创建docker用户组
默认情况下，docker 命令会使用 Unix socket 与 Docker 引擎通讯。而只有 root 用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 root 用户。因此，更好地做法是将需要使用 docker 的用户加入 docker 用户组。

#### 创建docker组
    groupadd docker

#### 将当前用户加入docker组
    usermod -aG docker $USER
这样每次使用docker就不用非得使用root用户或者sudo了

#### 启动docker
    systemctl enable docker
    systemctl start docker


---
### 网络设置
#### 打开网卡混杂模式
    sudo ip link set eth0 promisc on
    
    混杂模式（Promiscuous Model）:
    工作在混杂模式下的网卡接收所有的流过网卡的帧，抓包程序就是在这种模式下运行的。网卡的缺省工作模式包含广播模式和直接模式，即它只接收广播帧和发给自己的帧。如果采用混杂模式，网卡将接受同一网络内所有所发送的数据包，这样就可以到达对于网络信息监视捕获的目的。它将接收所有经过的数据包，这个特性是编写网络监听程序的关键。

#### 创建网络
    docker network create -d macvlan --subnet=192.168.31.0/24 --gateway=192.168.31.1 -o parent=eth0 macnet
    
    上面的网关网段要根据自己的实际情况配置,比如我的树梅派ip是192.168.31.253,所以subnet是192.168.31.0/24,网关192.168.31.1

#### 查看网络
    创建之后使用docker network ls 命令查看docker所有网络

    $  sudo docker network ls
    NETWORK ID          NAME                DRIVER              SCOPE
    5addd9978433        bridge              bridge              local
    588d43873bfc        host                host                local
    87f8ffd37be7        macnet              macvlan             local
    33fc1833eaa0        none                null                local

#### 导入镜像

编译固件可以参考之前的文章编译openwrt
    cat openwrt-bcm27xx-bcm2711-rpi-4-rootfs.tar.gz | docker import - openwrt-wind:1226	
    #最后的参数为镜像名和标签
#### 查看镜像
    docker images 	#查看镜像列表

    openwrt-wind                   1226                89c5edb75e9b        2 hours ago         184MB
    如以上镜像拉取成功了

#### 创建并启动容器
    docker run --restart always --name openwrt -d --network macnet --privileged openwrt-wind:1226 /sbin/init

    --restart always 为容器退出时始终重启,保证重启物理主机后容器能正常启动使用

    --name openwrt 容器别名,可以执行设置,方便区分使用

    -d 后台运行

    --network macnet 容器在之前创建的网络下运行

    --privilleged 特权模式下运行

    /sbin/init 定义容器启动后执行的命令

#### 查看容器运行状态
    docker ps -a	#查看容器运行状况,如下,状态为up则说明成功运行

    CONTAINER ID        IMAGE                                                     COMMAND             CREATED             STATUS              PORTS                    NAMES
    acb0b0bff88b        portainer/portainer                                       "/portainer"        3 days ago          Up 22 minutes       0.0.0.0:9000->9000/tcp   portainer
    915332f9a045        openwrt-wind:1226   "/sbin/init"        3 days ago          Up 22 minutes                                openwrt

#### 进入容器内修改网络参数
    docker exec -it openwrt-wind bash
    openwrt为容器名
    bash为进入容器后执行的命令

#### 修改openwrt的ip地址
```bash
vim /etc/config/network
将lan口配置如下:

config interface 'lan'
        option type 'bridge'
        option ifname 'eth0'
        option proto 'static'
        option netmask '255.255.255.0'
        option ipaddr '192.168.31.254'
        option gateway '192.168.31.1'
        option broadcast '192.168.31.255'
        option dns '114.114.114.114'
```

#### 配置完后重启网络
    /etc/init.d/network restart

接下来就可以通过web页面来访问并配置openwrt的具体内容了,可以参考之前的文章

### 解决宿主机和openwrt无法通信
当然了,如果是在宿主机上开启网卡混杂模式,然后启动了一个使用macnet的容器,这样容器和宿主机是无法通信的.关于macvlan
在一张物理网卡上虚拟出两个虚拟网卡，具有不同的MAC地址，可以让宿主机和docker同时接入网络并且使用不同的ip，此时docker可以直接和同一网络下的其他设备直接通信，相当的方便，但是这种模式有一个问题，宿主机和容器是没办法直接进行网络通信的，如宿主机ping容器的ip，尽管他们属于同一网段，但是也是ping不通的，反过来也是。因为该模式在设计的时候，为了安全禁止了宿主机与容器的直接通信，不过解决的方法其实也很简单——宿主机虽然没办法直接和容器内的macvlan接口通信，但是只要在宿主机上再建立一个macvlan，然后修改路由，使数据经由该macvlan传输到容器内的macvlan即可，macvlan之间是可以互相通信的。

#### 解决办法:

    假设宿主机ip 192.168.31.58,容器ip192.168.31.250
    ip link add mynet link eth0 type macvlan mode bridge
    # 为该接口分配ip，并启用
    ip addr add 192.168.31.10 dev mynet
    ip link set mynet up
    # 修改路由，使宿主机到192.168.31.250的通信全部经由mynet进行
    ip route add 192.168.31.250 dev mynet
    这样就可以ping通互相访问了.