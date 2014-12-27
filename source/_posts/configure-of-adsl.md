title: 关于ADSL拨号配置文件的设置
tags:
  - ADSL
  - Fedora
id: 67
categories:
  - Linux
date: 2007-04-15 12:45:34
---

PPPOE_TIMEOUT=80 #这个应该是连接后没有数据传输超时，超过这个时间就断线。这个值要设置的大一些

CONNECT_TIMEOUT=0 #此项为重新连接的间隔时间，当然是越小越好了。可以设置为0,就是只要断线不停的拨，呵呵，狠！

LCP_INTERVAL=5 #LCP是Link Control Protocol,这个值也小一点的好，但原因我不清楚

如果出现以下错误，很可能就是上面几项设置不合理的原因:

Inactivity timeout... something wicked happened on session XXXX

我在写一个脚本的时间把PPPOE_TIMEOU设置为6,CONNECT_TIMEOUT设置为4,结果拨号成功后马上就断开，并且出现上面的错误信息。