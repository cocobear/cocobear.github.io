title: Jwind完成［Java编写的一个论坛自动发贴机］
id: 137
categories:
  - 编程相关
date: 2007-06-16 01:25:34
tags:
---

针对phpwind 5.3版本，目前功能介绍：

*   在验证码没有开启的情况下可以实现自动注册并且发贴。

*   当注册验证码开启，登录验证码未开启的情况下可以手动注册，然后输入用户名与密码。

*   当注册验证与登录验证全部开启的情况下，本程序无任何作用。
这个星期做课程设计，就写了这个小程序，界面内容仿照晨风论坛灌水机，内容还包括发贴后自动回贴，回复已经存在的贴子，这两个还没有写，不过挺容易实现的，遵循release early的原则。Java代码写的不是很好，基本没有做什么异常处理。先完成课程设计的要求，以后再继续更新，以后会加入验证码分析、或者获得验证码，手动输入。当然更可能的是在程序里显示验证码，然后手动输入，因为毕竟验证码的分析不是一件很容易的事情。

第一个比较大的Java程序，写的时候遇到的很多问题，不过还好，一步一步走下来了，也按照预期的目标完成了该完成的内容。程序的实际意义并没有多少，不过学到了不少东西，GUI的设计（全部是手动写的代码），HTTP协议，httpclient这个包的使用，差点把这个忘了，我的前一篇文章提到了这个包的使用， 它提供了很方便的HTTP操作，可以使我们把程序的重点放在设计上，而不是麻烦的HTTP操作。不过我的程序用到的东西很少，即使用Java已有的java.net里面的类也是很容易实现，更多httpclient的信息可以在[HttpClient Home](http://jakarta.apache.org/commons/httpclient/)找到。

不过请**注意** [HTTPClient ](http://www.innovation.ch/java/HTTPClient)与上面说的httpclient是两个不同的项目，我所用的是[Apache ](http://www.apache.org)的一个开源项目，我刚开始的时侯就把这两个搞混了，结果在这上面浪费了不少时间:-(

[查看代码](http://cocobear.github.io/code/html/Jwind.html)

[代码下载 ](http://cocobear.github.io/code/Jwind.java)

遵循[GPL](http://www.gnu.org/licenses/licenses.zh-cn.html)发布