title: 关于发邮件的一些事
tags:
  - gmail
  - sendmail
id: 561
categories:
  - 互联网
date: 2009-02-27 16:50:47
---

今天配置一个Bug跟踪的工具，使用Sendmail发邮件时一直出错，先没仔细看，后来仔细看了下，在/var/log/maillog里面有出错提示：

     451 DT:SPM mx4, 7lcQrLDL7iJgl6dJktVcJQ--.7531S2, please try again 1235720034 http://mail.163.com/help/help_spam_16.htm?ip=1038939298&hostid=mx4&time=1235720034

在上面的链接中会有详细的提示，这个错误451：

     发信人短期内发送了大量信件，超过了网易的限制，该发件人被临时禁止发信。请检查是否有用户发送病毒或者垃圾邮件，并降低该用户发信频率。

郁闷啊，我才手动连着发了几封而已。还有不少地方需要注意。

还有如果直接使用sendmail向gmail发邮件会提示：

     http://mail.google.cn/support/bin/answer.py?hl=en&answer=10336> 
     
     'The IP you're using to send email is not authorized...'> 
     
     In order to prevent spam, Gmail refuses mail when the sending IP address does not match the sending domain. To send mail from your server to Gmail, we suggest using the SMTP relay provided by your ISP. Please note that we are unable to whitelist IP addresses or otherwise make exceptions at this time.

还是gmail考虑的多些，gmail的垃圾邮件过滤确实做的也不错。
## Comments

**[王硕](#5363 "2009-03-04 13:17:34"):** cocobear学长，我林晗烟。我想申请个友情链接。 你博客的链接我已经做好好久了，你可以查看：http://www.wangshuo2046.cn/?page_id=625 呵呵，不知道可不可以哈

