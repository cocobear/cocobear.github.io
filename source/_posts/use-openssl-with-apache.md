title: Apache使用Openssl实现HTTPS访问
tags:
  - openssl
id: 638
categories:
  - 互联网
date: 2009-05-08 12:35:29
---

首先确认apache支持mod_ssl， openssl也正确安装。

Fedora 8中apache关于ssl的配置文件在/etc/httpd/conf.d/ssl.conf文件中，使用SSLEngine on选项来开启HTTPS的支持，apache默认会使用一个自带的localhost 的证书来进行HTTPS连接。

接下来我们使用openssl生成自己的证书进行HTTPS连接：

	mkdir /etc/httpd/conf.d/ssl.crt/
	cd /etc/httpd/conf.d/ssl.crt
(1)
生成服务器的私钥：
	openssl rsa -in server.key -out server.key

	openssl req -new -key server.key -out server.csr
生成Certificate Signing Request（CSR）,生成的csr文件交给CA签名后形成服务端自己的证书.屏幕上将有提示,依照其指示一步一步输入要求的个人信息即可.

(2)
对客户端也使用同样的方式：
	openssl genrsa -out client.key 1024
	openssl req -new -key client.key -out client.csr

(3)
生成CA中心的私钥：
	openssl genrsa -out ca.key 1024
生成X509格式的CA证书：
	openssl req -new -x509 -keyout ca.key -out ca.crt

(4)
用生成的CA的证书为刚才生成的server.csr,client.csr文件签名:
	mkdir ../../CA
	mkdir ../../CA/newcerts
	touch ../../CA/serial
	cat "01" > ../../CA/serial
	touch ../../CA/index.txt
	openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key
	openssl ca -in client.csr -out client.crt -cert ca.crt -keyfile ca.key

(5)
生成浏览器使用的PFX格式的证书：
	openssl pkcs12 -export -in client.crt -inkey client.key -out client.pfx
把ca.crt安装到信任的机构，client.pfx安装或安装到个人证书位置

(6)
编辑ssl.conf配置文件，修改证书相关的地方：
	SSLCertificateFile /etc/httpd/conf.d/ssl.crt/server.crt
	SSLCertificateKeyFile /etc/httpd/conf.d/ssl.crt/server.key
	SSLCACertificateFile /etc/httpd/conf.d/ssl.crt/ca.crt
## Comments

**[kaiixin](#6781 "2009-12-03 16:03:28"):** 我日 第一个命令就不能执行 麻烦写清楚点

