---
layout: post
title: Linux多WAN负载均衡原理
tags: [network]
---


玩了一段时间 EdgeRouter , 对他的负载均衡的可玩性非常着迷。仔细研究后发现，他的实现原理是 iptables 标记数据包，然后对标记的数据包使用使用特定的路由表实现的。

Linux 默认有一个路由表。这张路由表叫 main 路由表。但是可以用 ip route 命令添加其他路由表。非默认路由表可以用一个数字编号。并且 ip rule fwmark 命令可以设定让被标记的数据使用这个路由表。

接着，接下来的工作就是，对数据包进行标记。

标记是基于 connection 的，否则对同一个tcp链接的包用不同的接口出去那可就乱套了。
基于 conntrak 链，对被跟踪的连接打标记。 打标记的时候，可以首先根据目的 ip 打标记，这样就可以避免有特定的ip跟踪的网站出现访问异常。
然后接下来的包，就用 probability 规则打标记. 也就是可能性。 这样有 50% 的新链接被打到 mark 1 上， 50% 的打到 mark 2 上。于是就有一半的流量走 table 1 , 一半的走 table 2

table 1 使用的 interface 1 做默认网关， table 2 用 interface 2 做默认网关。 于是2个接口上的流量就这样被平均过去了。



