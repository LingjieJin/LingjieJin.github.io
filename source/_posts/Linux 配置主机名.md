vi /etc/resolv.conf

在里面按下面格式添加后保存生效即可

nameserver 119.29.29.29

nameserver 8.8.8.8

临时修改主机名：

sudo hostname 新的主机名
1
永久修改主机名：

主机名存储在两个地方，两个地方都有修改

sudo nano /etc/hostname
1
将原本的名称改为新的主机名

sudo nano /etc/hosts
1
将原本的名称改为新的主机名