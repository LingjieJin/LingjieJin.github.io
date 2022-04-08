# Ubuntu 18 开启nfs服务

## 参考文章
[参考1](https://blog.csdn.net/weixin_30463341/article/details/96314065?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.pc_relevant_default&spm=1001.2101.3001.4242.1&utm_relevant_index=3)

[ubuntu18.04上安装和配置NFS服务器](https://www.cnblogs.com/mrld/articles/14149708.html)

[如何在Ubuntu 18.04上设置NFS挂载](https://www.howtoing.com/how-to-set-up-an-nfs-mount-on-ubuntu-18-04)

[Ubuntu18.04 NFS 安装使用](https://www.lockey.xyz/2020/11/12/ubuntu18-04-nfs-%E5%AE%89%E8%A3%85%E4%BD%BF%E7%94%A8/)

[如何在Ubuntu 18.04上安装和配置NFS服务器](https://cn.joecomp.com/how-install-configure-an-nfs-server-ubuntu-18)


## 安装
```
sudo apt-get install nfs-kernel-server -y
```

## 配置
nfs的配置文件 /etc/exports
里面有example

```
增加策略，每个策略一行
设定格式如下：
        共享目录 主机名称或者IP(参数1, 参数2)

/nfs *(rw,sync,no_root_squash)
 
-------------------------------------------------------
/nfs                             ：要共享的路径
*                                  ：*通配，表示所有网段都可以访问
sync                            ：同步写入硬盘
no_root_squash     ：nfs客户端共享目录使用者权限
```
## 测试
```
启动
service nfs-kernel-server restart

停止
service nfs-kernel-server stop

查看
service nfs-kernel-server status
或者
netstat -a | grep nfs
或者
nfsstat 
```

## nfs客户端


## nfs常用参数

### NFS的常用目录及常用命
```
/etc/exports NFS服务的主要配置文件
/usr/sbin/exportfs NFS服务的管理命令
/usr/sbin/showmount 客户端的查看命令
/var/lib/nfs/etab 记录NFS分享出来的目录的完整权限设定值
/var/lib/nfs/xtab 记录曾经登录过的客户端信息
```
### NFS配置
NFS服务的配置文件为 /etc/exports，这个文件是NFS的主要配置文件，不过系统并没有默认值，所以这个文件不一定会存在，可能要使用vim手动建立，然后在文件里面写入配置内容。
```
/etc/exports文件内容格式：
<输出目录> [客户端1 选项（访问权限,用户映射,其他）] [客户端2 选项（访问权限,用户映射,其他）] 
这里的参数有两部分－－－服务端参数、客户端参数。
a. 输出目录：
输出目录是指NFS系统中需要共享给客户机使用的目录；
b. 客户端：
客户端是指网络中可以访问这个NFS输出目录的计算机
客户端常用的指定方式
指定ip地址的主机：192.168.0.200
指定子网中的所有主机：192.168.0.0/24 192.168.0.0/255.255.255.0
指定域名的主机：david.bsmart.cn
指定域中的所有主机：*.bsmart.cn
所有主机：*
c. 选项：
选项用来设置输出目录的访问权限、用户映射等。
NFS主要有3类选项：
访问权限选项
设置输出目录只读：ro
设置输出目录读写：rw
用户映射选项
all_squash：将远程访问的所有普通用户及所属组都映射为匿名用户或用户组（nfsnobody）；
no_all_squash：与all_squash取反（默认设置）；
root_squash：将root用户及所属组都映射为匿名用户或用户组（默认设置）；
no_root_squash：与rootsquash取反；
anonuid=xxx：将远程访问的所有用户都映射为匿名用户，并指定该用户为本地用户（UID=xxx）；
anongid=xxx：将远程访问的所有用户组都映射为匿名用户组账户，并指定该匿名用户组账户为本地用户组账户（GID=xxx）；
其它选项
secure：限制客户端只能从小于1024的tcp/ip端口连接nfs服务器（默认设置）；
insecure：允许客户端从大于1024的tcp/ip端口连接服务器；
sync：将数据同步写入内存缓冲区与磁盘中，效率低，但可以保证数据的一致性；
async：将数据先保存在内存缓冲区中，必要时才写入磁盘；
wdelay：检查是否有相关的写操作，如果有则将这些写操作一起执行，这样可以提高效率（默认设置）；
no_wdelay：若有写操作则立即执行，应与sync配合使用；
subtree：若输出目录是一个子目录，则nfs服务器将检查其父目录的权限(默认设置)；
no_subtree：即使输出目录是一个子目录，nfs服务器也不检查其父目录的权限，这样可以提高效率；
```

### 客户端参数
```
先来看服务端在/etc/exports 括号中可以指定的参数：
选项
描述
rw
允许读写权限
ro
只读权限
sync
同步模式(Default)，所有数据在写入后可以请求
async
异步模式，数据在写入过程中可以写入
secure
NFS通过1024以下的安全TCP/IP端口发送(Default)
insecure
NFS可以通过所有端口发送
wdelay
如果多个用户要写入NFS目录，则归组写入 (Default)
no_wdelay
如果多个用户要写入NFS目录，则立即写入，当使用async时，无需此设置。
subtree_check
如果共享/usr/bin之类的子目录时，强制NFS检查父目录的权限 (Default)
no_subtree_check
与subtree_check对应
root_squash
Map requests from uid/gid 0 to the anonymous uid/gid.
no_root_squash
root用户具有根目录的完全管理访问权限。
all_squash
Map all uids and gids to the anonymous user. 对公共目录访问时较有用。
no_all_squash
保留共享文件的UID和GID (Default)
anonuid=UID
指定匿名用户访问时映射机的用户uid
anongid=GID
指定匿名用户访问时映射机的用户gid
```

### NFS热重启
```
如果我们在启动了NFS之后又修改了/etc/exports，是不是还要重新启动nfs呢？这个时候我们就可以用exportfs命令来使改动立刻生效，该命令格式如下：
exportfs [-aruv]
-a ：全部mount或者unmount /etc/exports中的内容
-r ：重新mount /etc/exports中分享出来的目录
-u ：umount 目录
-v ：在 export 的时候，将详细的信息输出到屏幕上。
具体例子：
[root @test root]# exportfs -rv  （全部重新export一次！）
exporting 192.168.0.100:/home/test
exporting 192.168.0.*:/home/public
exporting *.the9.com:/home/linux
exporting *:/home/public
exporting *:/tmp
reexporting 192.168.0.100:/home/test to kernel
具体例子：
[root @test root]#exportfs -au （全部都卸载了）
[root @test root]# vi /etc/exports
 
/home/soft 192.168.2.11(rw)
[root@localhost init.d]# nfs start
-bash: nfs: command not found
[root@localhost init.d]# ./nfs start
Starting NFS services: [ OK ]
Starting NFS quotas: [ OK ]
Starting NFS daemon: [ OK ]
Starting NFS mountd: [ OK ]
```