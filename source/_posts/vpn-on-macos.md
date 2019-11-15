---
title: macOS上使用L2TP+IPSec访问内网资源
tags: [macos,vpn]
toc: true
comments: true
date: 2019-11-14 19:44:55
categories:
description: 需要从家里访问公司内网的资源，很自然想到了使用VPN，记录一下。
---

## VPN服务端安装(L2TP + IPSEC VPN)

服务器是`CentOS 7`,为了方便使用了`docker`安装：
```
docker run --name ipsec-vpn-server --env-file /etc/l2tp-env --restart=always -p 500:500/udp -p 4500:4500/udp -v /lib/modules:/lib/modules:ro -d --privileged hwdsl2/ipsec-vpn-server

```
路由器上打开了500和4500的端口映射。

## macOS 使用

在`macOS`的网络设置里面直接新建一个VPN的连接，输入VPN的相关配置。

然后试着访问一个内网的服务器结果不行，拿出Android手机试了一下，发现是可以的，想着应该是`macOS`下的配置问题，google一下看到有人提到了在VPN配置的高级选项里可以勾选 `通过VPN连接发送所有流量`, 确实可以解决问题。

不过我又想既然默认`macOS`没有让所有连接都走VPN，那何不加个路由规则去让公司内网的IP走VPN,这样不影响其它网络访问。

查看当前的路由：
```
 route -n get 192.168.0.12
```
发现果然没有走VPN对应的网关， 添加新的路由：
```
sudo route add 192.168.1.0/24 192.168.42.1
sudo route add 192.168.0.0/24 192.168.42.1
```
果然可以访问了。 因为公司有两个网段，所以加了两条路由。

不需要的话删除该路由
```
sudo route delete 192.168.0.0/24
```


同时也发现了一个`android`上面使用VPN的一个问题， `android`上面可以访问第一个网段，第二个不行，VPN的服务是安装在一台虚拟机上面，该机器是0网段，但这个物理机器又是1网段，不太确定和这个有关系没，估计应该也是路由的问题。


