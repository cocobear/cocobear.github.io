title: SSL SSL2.0 SSL3.0 TLS 1.0
tags:
  - openssl
  - ssl
id: 641
categories:
  - 互联网
date: 2009-05-08 20:54:39
---

SSL是用于网络安全传输的协议，在TCP之上，HTTP之下。以前对标题里面这几个有点分不清楚，今天花了点时间整理下概念。

SSL为(Secure Sockets Layer)的缩写，是网景公司为网络安全传输制定的一套标准，SSL1.0没有公开发布过，所以SSL1.0可以无视之。SSL2.0在1995年发布，不过因为有很多的安全漏洞，所以SSL3.0很快在1996年就出现了。不过主流的浏览器在很长的一段时间内都在支持SSL2.0,IE6默认是支持SSL2.0的(IE7中SSL2.0被禁用了)，Firefox2以后禁用了SSl2.0,Opera在8.5以后也禁用了SSL2.0。

IETF在1999年的时候以SSL3.0为基础，制定了TLS(Transport Layer Security) 1.0，[RFC2246](http://tools.ietf.org/html/rfc2246)对TLS进行了详细的描述。TLS 1.0在框架上完全使用SSL3.0，只是在一些细节上有差异，比如采用的算法集，随机函数的产生，[这里](http://blog.ixpub.net/html/03/12684303-24382.html)有一篇文章对TLS与SSL3.0区别进行了详细的描述。
由于TLS基本上是对SSL3.0的补充，因此在很多地方SSL3.0 TLS这两个名词被混合使用。

TLS也在更新，不过变化不大，[RFC4346](http://tools.ietf.org/html/rfc4346)对TLS 1.1进行了详细描述。
TLS最新版本为1.2，相关的RFC为[RFC5246](http://tools.ietf.org/html/rfc5246)。

目前国内网站使用的HTTPS其本上都是基于SSL3.0的。

以下是wireshark抓包一个SSL3.0的通信过程：

	client  ------------->server [Client Hello]
	server ------------>client   [Server Hello]
	server ------------>client   [Certificate]
	client  ------------->server  [Client Key Exchange , Change Cipher Spec, Encrypted Handshake Message
	server ------------->client  [Change Cipher Spec, Encrypted Handshake Message
	client -------------->server [Application Data]
## Comments

**[可可熊](#5892 "2009-05-11 14:51:16"):** 最近在做PKI相关的东西，公司的事。

**[草儿](#5876 "2009-05-09 00:20:34"):** 怎么从pythunder突然转向SSL了？难道……

**[Kermit](#5887 "2009-05-11 11:18:08"):** 你最近在做那方面的工作？感觉你blog上很多东西的跨度确实有点大……

**[rhlei](#5884 "2009-05-11 07:43:55"):** DP做的东西面真广~~哈哈

**[可可熊](#5885 "2009-05-11 09:19:20"):** 不止这些。。。

