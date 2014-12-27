title: 重新把代理打开了
tags:
  - GFW
  - Linux
  - proxy
id: 231
categories:
  - Linux
date: 2007-12-07 09:38:36
---

还是原来的地址
http://cocobear.github.io/gfw
以前大概是因为被太多人用了，所以导致DH把这个给我封了，但后来用其它代理一点也不习惯，只好重新开始用自己的代理。不过这次加入了验证功能，一般人访问不了，如果谁想用这个代理可以发邮件给我，我告诉你用户名，密码。

说下配置方法：
首先在gfw这个需要被保护的文件夹下面建一个文件.htaccess，添加以下内容：

	AuthUserFile    /home/cocobear/cocobear.cn/gfw/.htpasswd
	AuthName        "Please contract cocobear"
	AuthGroupFile   /home/cocobear/cocobear.cn/gfw/
	AuthType        Basic
	require         valid-user


挺好理解的，就不多解释了，require选项是指定允许访问的用户，valid-user指所有有效的用户，也可以指定某些用户。

接下来是生成.htpasswd文件，需要用htpasswd这个工具来向.htpasswd添加新用户名：

htpasswd -m .htpasswd _username_

输入命令后会要求为该用户输入密码。这样是生成单个用户，其它批量生成用户可以查看htpasswd的手册，密码是经过md5加密的。

好了，这才算是专用代理。
## Comments

**[crazyfranc](#2603 "2007-12-08 10:22:39"):** 不错，我也给自己加一个；）

**[Amankwah](#2604 "2007-12-08 11:08:49"):** 好，我准备用～

**[Notice](#2607 "2007-12-08 14:47:10"):** 记得要把密码加上，不然被搜索比引擎收录了就麻烦了。

**[草儿](#2612 "2007-12-08 22:19:10"):** 呵呵，要个帐户用用先

