# 设置云服务器与本地客户端SSH免密登录

云服务器操作系统：Ubuntu 16.04
本地k客户端系统：windows子系统 Ubuntu 18.04

# 查看云服务器SSH-server是否启动
```
1. 安装
apt-get install openssh-server

2. 查看状态
service ssh status

2. 修改ssh-server设置
nano /etc/ssh/sshd_config // ssh_config是client设置 sshd_config是server配置
```

## 设置流程

```
1.地客户端生成ssh密钥
进入当前系统的用户主目录
cd ~

2. 生成公私密钥
ssh-keygen
一直回车操作，就会在当前目录下生成.ssh文件夹
在.ssh文件夹中有两个文件：
id_rsa(私钥)
id_rsa.pub(公钥)

3. 上传公钥到服务器
ssh-copy-id -i ~/.ssh/id_rsa.pub root@服务器ip地址(xxx.xxx.xxx.xxx)
将客户端的公钥写入到服务器root用户目录下的.ssh文件夹中
可以在服务器中查看：
cd ~/.ssh
cat authorized_keys

4. 完成 测试免密登录
```

# 设置云服务器SSH连接持久化

通过ssh连接阿里云服务器总是在一段时间后自动断线
设置一下ssh连接参数，解决这个糟心事

通过配置云服务器端的ssh服务
```
1. nano /etc/ssh/sshd_config
在这里寻找两个字段的设置
ClientAliveInterval 60     #服务端主动向客户端请求响应的间隔
ClientAliveCountMax 10    #服务器发出请求后客户端没有响应的次数达到一定值就自动断开

不过在我的主机上并没有找到这两个字段，所以直接自己加入。(在自己本地的ubuntu找到了这两个字段，怀疑是云服务器ssh设置拉掉了这两项，可能是安全考虑吧)

然后重启ssh服务
/etc/init.d/ssh restart 
这里有个问题 为啥阿里云的ubuntu没有sshd服务？？？
How to Enable SSH in Ubuntu 16.04 LTS
ref:http://ubuntuhandbook.org/index.php/2016/04/enable-ssh-ubuntu-16-04-lts/
```


！！！https://www.rosehosting.com/blog/how-to-install-and-configure-openssh-on-ubuntu-16-04/


# 在阿里云服务器上搭建Git服务

阿里云系统：ubuntu 16.04

参考文章：
1.https://www.cnblogs.com/dst5650/p/8503772.html

2.https://www.cnblogs.com/herd/p/7063091.html

3.https://www.cnblogs.com/polo/p/5525988.html

4.https://yq.aliyun.com/articles/708262

```
## 1.安装git
apt-get install git

## 2.创建git用户和git用户组
adduser git
groupadd git-server

# 将git用户加入git-server组中
addgroup git git-server

## 3.在客户端创建ssh密钥(参考ssh密钥一文)

## 4.将客户端的公钥保存到服务器端

## 5.在服务器端初始化git仓库
# 选择一个文件夹作为git的仓库入口
# 比如 /home/git/gitroot/
# 这里需要对文件夹的所有权进行修改
chown git:git-server gitroot/ ## chown将指定文件的拥有者改为指定的用户或组
                              ## chown [选项]... [所有者][:[组]] 文件...

## 6.创建一个git仓库测试
cd gitroot
git init --bare test.git
chown git:git-server test.git ## 这步一定需要，因为当前是root用户，之前创建的文件是root权限，导致下一步git push写入不了

## 7.客户端clone，push操作测试
git clone git@xxx.xxx.xxx.xxx:/home/git/gitroot/test.git

touch test.txt
git add .
git commit -m "test"
git push

## 8.完成搭建
```


## 问题
之前翻阅资料时候，参考2文章提到关闭git账户的ssh访问权限，只开放git-shell访问，但是那样子设置后，git clone时一直提示访问拒绝。
通过参考4的链接，发现是之前博主的设置没有写清楚。
设置git-shell需要确认本机的git-shell位置，比如我这个机器地址是/usr/bin/git-shell
所以要根据自己本机的位置进行设置

