---
title: Linux 设置nfs服务
date: 2022-04
tags: [linux, nfs]
---

# Linux 设置nfs服务

#### 安装nfs服务
sudo apt-get install nfs-kernel-server -y

#### 安装NFS 客户端
sudo apt-get install nfs-common

#### 修改配置文件
sudo vi /etc/exports

权限属性：

    ro:只读

    rw:读写

    sync:同步，数据同步写到内存与硬盘中

    async:异步，数据先暂存内存

    root_squash: 将root用户映射为来宾账号

    no_root_squash: 有root的权限，不建议使用

    all_squash: 全部映射为来宾账号

    anonuid, anongid: 指定映射的来宾账号的UID和GID

##### exportfs命令：

    -a：跟-r或-u选项同时使用，表示重新挂载所有文件系统或取消导出所有文件系统；

    -r: 重新导出

    -u: 取消导出

    -v: 显示详细信息

##### showmount命令：

showmount -e NFS_SERVER: 查看NFS服务器"导出"的各文件系统

showmount -a NFS_SERVER: 查看NFS服务器所有被挂载的文件系统及其挂载的客户端对应关系列表

showmount -d NFS_SERVER: 显示NFS服务器所有导出的文件系统中被客户端挂载了文件系统列表


##### rpcinfo

    -p hostname(orIP)

    -p ：显示所有的 port 与 program 的信息！

 

    如果要让mountd和quotad等进程监听在固定端口，编辑配置文件/etc/sysconfig/nfs


在最后一行添加：/home/nfs (rw,sync,no_root_squash,no_subtree_check)

    前面那个目录是与nfs服务客户端共享的目录，代表允许所有的网段访问（也可以使用具体的IP）
    rw：挂接此目录的客户端对该共享目录具有读写权限sync：资料同步写入内存和硬盘
    no_root_squash：客户机用root访问该共享文件夹时，不映射root用户
    root_squash：客户机用root用户访问该共享文件夹时，将root用户映射成匿名用户
    no_subtree_check：不检查父目录的权限。

 

#### 客户端使用mount命令挂载

mount -t nfs NFS_SERVER:/PATH/TO/SOME_EXPORT /PATH/TO/SOMEWHRERE

#### 重启nfs服务
sudo service nfs-kernel-server restart


#### 挂载目录
mount -n -o nolock xxx.xxx.xxx.xxx:/nfs在服务器端的地址（使用绝对路径） /挂载目录