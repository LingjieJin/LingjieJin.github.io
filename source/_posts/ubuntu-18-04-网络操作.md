# Ubuntu 1804 网络配置有关

## 基础配置
### 修改主机名
   
    hostnamectl set-hostname ubuntu1804


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


## 查看网卡
lspci -v 查看网卡信息

