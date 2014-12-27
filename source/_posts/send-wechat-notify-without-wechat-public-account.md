title: 在没有微信公众账号的情况如何发送微信通知
id: 1230
categories:
  - Linux
date: 2013-12-19 11:25:40
tags:
---

如果你有一个需求，对某个事件进行监控，而在出现异常的情况下需要通知自己，通知的方式最常用的应该是短信、电话、微信等，而短信、电话用起来不是很方便，目前微信比较流行，所以可以考虑使用微信通知。

但是微信公众账号申请太麻烦，这里提供一个最简单的通知办法，微信可以收到QQ的邮件通知，所以如果我们需要发微信通知的时候，只需要给你微信号所绑定的QQ邮箱发一个邮件就可以了。

下面是Linux下发送邮件的一个简单的办法，使用了外部SMTP服务器发方式发送，这样可以避免发送邮件时被邮件服务商当作垃圾邮件处理。
* 安装mail 
* 配置 /etc/mail.rc 
* 添加以下内容 
    
    
     set from=123456@qq.com
     set smtp=smtp.qq.com  
     set smtp-auth-user=123456
     set smtp-auth-password=sbsbsbsb
     set smtp-auth=login 
    

* 然后通过命令来发送邮件 


     echo  hello  | mail -s " title" 123456@qq.com