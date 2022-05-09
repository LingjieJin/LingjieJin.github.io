---
title: IP V6测试
date: 2022-05
tags: [ipv6]
---

# IP V6测试

### windows 查看ipv6信息
通过*ipconfig-all*命令查看到ipv6地址如下：
    
    fe80::cd04:c16b:9adf:dfe7%22

%后面是本ipv6地址对应的网络接口的index，windows术语叫scope id，可理解为一个接口序号，则22为当前windows接口序号。

### linux 查看ipv6信息
通过*ip addr*命令查看到ipv6地址如下：
    
    fe80::20c:29ff:fea4:1509

### windows主机 ping linux的IPv6地址
    命令：ping -6 （linux的ipv6地址）%（windows的接口序号）

### linux主机 ping windows的IPv6地址
    命令：ping6 -I linux_接口名 win_ipv6地址

### linux主机 ping linux的IPv6地址
    命令：ping6 –I A服务器linux接口名 B服务器linux_ipv6地址