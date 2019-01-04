---
title: iTerm2使用代理
date: 2019-01-03 16:29:45
tags: [iterm2,macos,proxy,brew]
categories: MacOS
toc: true
comments: true
description: 从AWS上面下载东西太慢了，想通过代理下载，结果一直不成功
---

<img src="https://asset-1258390188.cos.ap-shanghai.myqcloud.com/iterm2-proxy.gif"/>

<!--more-->

##遇到的问题

以前在`Windows`下一直用`ShadowsocksR`科学上网，平时是不会开全局代理，只是在`Chrome`里面用插件`Proxy SwitchyOmega`配置成`auto switch`模式，基本可以满足日常使用，偶尔其它软件有要科学上网的需要，就打开`ShadowsocksR`的全局模式。

这样用着一直很顺，直到今天在`MacOS`下面安装软件时发现有个软件在亚马逊的`S3`上面，下载速度奇慢(*在家里电信的网络却没有遇到这问题*)，于是很自然的就打开`ShadowsocksX-NG`的全局模式，结果在`iTerm2`下面`brew`速度依然很慢，试着重新打开`iTerm2`，也不行，折腾了一会儿都不行，突然意识到是不是`ShadowsocksX-NG`的全局模式没有起作用，于是测试了一下：

```shell 
curl ip.sb
111.18.41.235
```

果然是国内的IP地址，后来又了解了一下，原来`MacOS`下`ShadowsocksX-NG`全局代理在`iTerm2`里是不起作用的。

## 解决方案

通过`Google`了解到可以通过设置环境变量来手动指定终端下面的代理，于是手动进行设置环境变量`ALL_PROXY`，最后`brew`下载速度果然上去了。

```shell 
export ALL_PROXY=socks5://127.0.0.1:1080
curl ip.sb
66.112.220.gfw

```

下载完成后，恢复原来的环境：

```shell 
unset ALL_PROXY
curl ip.sb
111.18.41.235
```

