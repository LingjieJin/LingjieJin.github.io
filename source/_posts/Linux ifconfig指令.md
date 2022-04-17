---
title: Linux ifconfig 指令
date: 2022-04
tags: [linux, network, ifconfig, 指令]
---

# ifconfig命令

ifconfig命令:（立即生效,重启后失效）

    ifconfig [interface（接口）]    \\查看某某接口
    ifconfig -a                     \\查看所有接口（包含未生效的接口）
    ifconfig 接口 [up|down]         \\开启或禁用接口
    ifconfig 接口 IP/netmask [up]   \\配置接口的IP地址网关并开启接口
    ifconifg 接口  IP netmask NETMASK   \\配置接口的IP和子网掩码

