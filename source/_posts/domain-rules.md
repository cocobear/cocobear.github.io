title: 规范域名
tags:
  - Domain
id: 139
categories:
  - 互联网
date: 2007-06-18 13:05:23
---

 一个网站同时使用www.domain.cn与domain.cn两个域名会对搜索引擎收录网站内容造成一些不必要的麻烦，具体可查看这篇文章：

[URL网址规范化 ](http://www.chinamyhosting.com/seoblog/2006/04/10/url-canonicalization/)

如果你看了这篇文章打算规范你的URL（这也正是我遇到的问题），那么你可以试着在.htaccess文件中使用下面的代码：
     Options +FollowSymLinks> 
     RewriteEngine on> 
     RewriteCond %{HTTP_HOST} www.cocobear.cn> 
     RewriteRule ^(.*)$ http://cocobear.github.io/$1 [R=301,L]
这段代码可以把从www.cocobear.cn来的访问全部重定向到cocobear.cn（使用301永久转向），更重要的是使用301定向可以使搜索引擎明白www.cocobear.cn永久转向cocobear.cn,同时可以转移PR值。

有关301的更多内容可以查看[301转向和网址规范化](http://www.chinamyhosting.com/seoblog/2006/04/12/301-redirect)

有一点需要注意的是.htaccess文件只适合于apache服务器，对于IIS则需要在管理面板中更改301重定向。
## Comments

**[草儿](#360 "2007-06-18 21:17:49"):** 感觉没啥用啊～

**[Amankwah](#372 "2007-06-19 22:44:38"):** 不知道～

**[windflush](#377 "2007-06-21 17:53:41"):** 更新一下好吗

**[cocobear](#378 "2007-06-21 19:09:43"):** 好啊,那你替我想想写什么啊?

