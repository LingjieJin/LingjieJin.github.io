# TFFP 服务器安装

TFTP是TCP/IP协议族中的一个用来在客户机与服务器之间进行简单文件传输的协议，以下讲解如何在ubuntu下安装配置tftp：
```
$ sudo apt-get install tftp-hpa tftpd-hpa

$ mkdir ~/tftpboot

$ chmod 777 ~/tftpboot/

$ sudo gedit /etc/default/tftpd-hpa

/etc/default/tftpd-hpa 参数

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/zifeng/tftpboot"    //tftpboot绝对路径
TFTP_ADDRESS=":69"
TFTP_OPTIONS="--secure  -l -c -s"

$ service tftpd-hpa restart
```

## 配置服务端
修改 /etc/default/tftpd-hpa 文件内容如下
```
# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/qiushao/tftp-root"
TFTP_ADDRESS=":69"
TFTP_OPTIONS="-l -c -s"
TFTP_DIRECTORY : tftp 启动根目录，　修改成自己想要的目录

TFTP_OPTIONS : tftp 启动选项，各选项解析如下：
-l –Listen
-c –create
-s –secure
```

如果你在客户端获取文件时出现 Error code 1: File not found 这个错误，请检查一下 TFTP_OPTIONS="-l -c -s"。
配置好后，重启 tftp 服务:

    $ sudo service tftpd-hpa restart


## 测试tftp获取文件
在/home/qiushao/tftp-root 目录下创建一个文件 foobar，
然后在 /home/qiushao　目录执行 tftp get 来下载文件:
```
$ cd tftp-root/
$ touch foobar
$ ls
foobar
$ cd 
$ tftp localhost
tftp> get foobar
$ ls foobar 
foobar
$
tftp get 下载文件成功了，说明我们的 tftp 服务应该是没有问题的了。
```
