---
title: Linux ss 指令
date: 2022-04
tags: [linux, network, ss, 指令]
---

# Linux ss命令
ss命令和netstat实现的功能相似，而且netstat的选项ss都可以用。

ss还有很多netstat没有的功能，并且，在用户连接数很多的时候，ss命令可以快速的显示，效率更高。netstat通过遍历proc来获取socket信息;ss使用netlink与内核tcp_diag模块通信获取socket信息

格式：

    ss [OPTION选项]... [FILTER过滤]

    [OPTION选项]：
       * -t: tcp协议相关
       * -u: udp协议相关
       * -w: 裸套接字相关
       * -x：unix sock相关
       * -l: listen状态的连接
       * -a: 所有
       * -n: 数字格式
       * -p: 相关的程序及PID
       * -e: 扩展的信息
       * -m：内存用量
       * -o：计时器信息

    常用组合：-tan, -tanl, -tanlp, -uan

    FILTER : [ state TCP-STATE ] [ EXPRESSION ]

    1. [ state TCP-STATE ]:TCP的各种状态表达式
        TCP的常见状态：
            tcp finite state machine(有限状态机共11种):

                - LISTEN: 监听
                - ESTABLISHED：已建立的连接
                - FIN_WAIT_1
                - FIN_WAIT_2
                - SYN_SENT
                - SYN_RECV
                - CLOSED
    
    2. [ EXPRESSION ]
    
        - dport(目标端口) =
        - sport(源端口) =
        - 示例：'(dport = :ssh or sport = :ssh)'目标端口或者源端口是ssh
