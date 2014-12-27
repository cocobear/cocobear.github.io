title: PHP中使用openssl
tags:
  - openssl
  - PHP
id: 716
categories:
  - Life
date: 2009-06-17 15:23:36
---

服务器端要对客户端POST 上来的签名后的数据进行验证，本来想着用Python调用openssl命令行来实现，不过没有找到相关的命令，后来找了下PHP的openssl的封装，果然有的：[http://php.chinaunix.net/manual/zh/ref.openssl.php](http://php.chinaunix.net/manual/zh/ref.openssl.php)

封装的函数也比较好用。（我还想着要是实在不行就用C语言写个CGI来调用openssl 了）

上面的代码中，首先从客户的证书中获取公钥，然后使用openssl_verify($data, $signature, $pubkeyid);对签名后的数据($signature)进行验证,$date为原始数据，$pubkeyid是从证书中读出的公钥，该函数还可以使用第四个参数，设置一个使用的hash算法，默认使用的是sha1。

被注释掉的代码是使用私钥对数据进行签名。

代码中第一行是获取原始的POST数据内容，也就是获取标准输入的数据。如果是Python写CGI的话可以使用:
data =  sys.stdin.read()

代码见：[http://code.google.com/p/cocobear/source/browse/trunk/openssl/openssl_php.php](http://code.google.com/p/cocobear/source/browse/trunk/openssl/openssl_php.php)

在wordpress日志里加PHP代码一直出错，没办法，只能放在别的地方。
## Comments

**[卢松松](#6182 "2009-06-27 17:00:56"):** 牛啊 程序高手！~以后多多交流啊！

**[冬瓜蛋汤](#6687 "2009-11-10 11:55:21"):** 那个google code的链接没有权限访问哦。。 能不能开下权限，谢谢！

