---
title: Linux netstat 指令
date: 2022-04
tags: [linux, network, netstat, 指令]
---

# netstat命令

格式

    netstat [--tcp|-t] [--udp|-u] [--raw|-w] [--listening|-l] [--all|-a] [--numeric|-n] [--extend|-e[--extend|-e]] [--program|-p]
    
    -t: tcp协议相关
    -u: udp协议相关
    -w: raw socket相关
    -l: 处于监听状态
    -a: 所有状态
    -n: 以数字显示IP和端口
    -e：扩展格式
    -p: 显示相关进程及PID
    -r: 显示内核路由表

1. 示例

    常用组合：-tan, -uan, -tnl, -unl

    显示路由表：netstat {--route|-r} [--numeric|-n]

    显示接口统计数据：

    netstat {--interfaces|-I|-i} [iface] [--all|-a] [--extend|-e] [--program|-p][--numeric|-n]

2. 示例
- 当前网络的连接情况(已连接的)              netstat -nt
- 当前网络的连接情况(连接和没连接的网络)     netstat -ntua
- 监听当前程序使用的端口                    netstat -nlt
- 查看某端口被什么程序使用                  netstat -nltp
    
    示例：查看端口6000被什么程序使用(面试题)，有两种方法：
        方法一(推荐)：lsof -i :6000
        方法二：netstat -nltp :6000

- 详细查看某端口被什么程序使用              netstat -nltpe
- 网卡的连接情况 : netstat  -i
    
    Iface(网卡名)、MTU(MTU的大小)、RX-OK(接收的数据报文)、TX-OK(发送的数据报文)
    通过“watch -n 0.5 netstat -i”可观察网卡在0.5秒内使用的情况
    “watch -n 0.5 netstat -IechX”可观察网卡echX在0.5秒内使用的情况