---
title: Hexo && Next升级
date: 2018-12-28 14:19:17
tags: [hexo, next, blog]
categories: 互联网
toc: true
comments: true
description: 好久没有更新博客了，给窝重新装修一下吧！
image:
---

![](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/hexo_next.jpg)
<!--more-->

## 起因

很久没有更新博客， 想记录些东西，看到`Hexo`版本比较老了，就顺便升级一下。

升级的方式有很多种，建议使用增量升级的方式，简单来说就是直接在新的环境中安装最新版本的`Hexo`以及`NexT`主题，然后创建一个博客的框架，把旧的配置文件中自定义的内容复制到新的配置文件中，最后把文章拷到新的目录中。
此次升级的目标版本为：

> hexo: 3.8.0
> hexo-cli: 1.1.0
> node: 10.13.0
> hexo-theme-next: 6.6.0

旧的版本

> Hexo: xxx
>
> hexo-theme-next: 5.1.0

## 升级步骤

```shell 
npm i hexo-cli -g
hexo init blog && cd blog
npm install hexo —save
npm install hexo-server --save
vim _config.yml theme/next/_config.yml
hexo clean && hexo g && hexo s
```





## 升级过程中问题记录

### 页面空白，只有侧边栏

有可能是用了motion, 解决办法修改Next的配置文件:

```yaml
motion:
  enable: false
```

也有可能是网络问题，刷新后会显示，但我用不到，所以就直接把这个关掉了。



### 导航栏不显示图标

![](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/no_icon.png)原因是一般和font-awesome.css 文件有关，比如我遇到的情况是chrome浏览器使用了Stylish 插件，自定义了自体样式，所以导致了图标未能正常显示，解决办法是让自定义的样式不要修改class为icon的样式：

```css 
body {text-shadow: #707070 0.05px 0.05px 0.05px}
*:not([class*="icon"]):not(i)
{font-family:Arial,"Microsoft YaHei" !important;}
*:not([class*="icon"]):not(i)
{font-weight:400!important;}
@font-face {font-family: "Microsoft YaHei";src: local("Source Han Sans Bold")}
@font-face {font-family: "Arial";src: local("Source Han Sans Bold")}
```



### 导航栏里面是英文

如上图， Home等未被汉化， 原因是新的Next主题里的汉化文件名有变化：

```diff 

-next/languages/zh-Hans.yml
+next/languages/zh-CN.yml
```

所以需要修改配置文件_config.yml

```diff
-language: zh-Hans
+language: zh-CN
```

### 导航栏的标签等不能点击

原因是改了hexo的配置文件, 不清楚的时候最好这里使用false：

```yml 
relative_link: false
```

## 图床迁移

以前用的七牛图床，突然有一天就挂了，当然不至于是七牛挂了，而是针对我们这些免费用户发了个邮件

> 测试域名回收通知
>

感觉七牛做的不太地道，直接回收了域名，都没有办法续用，网上很多人在用静态博客的时候都用了七牛的图床，最近几个月看到好多人博客的图片挂掉了。七牛这么做让很多人不舒服，当然毕竟是免费使用，大家也无话可说，所以很多人干脆就换地方了。

如果要继续使用七牛还需要进行实名认证以及使用国内备案的域名，我不想弄，所以干脆换个地方，还好有办法可以把图片导出，参考[这篇文章](https://blog.csdn.net/lkj345/article/details/83382636)。找了找各种图床，最后还是决定用腾讯的COS，首先免费，再次速度还不错，参考[腾讯云作为Hexo图床](https://www.yangyilts.com/2018/使用腾讯云作为Hexo图床.html/)把七牛的图片都导入了腾讯。然后就是使用`sed`把文章里面的链接替换掉，好久没用`sed`, 在写替换命令时费了老长时间。打算把图片名字后缀修改一下，比如`lnmp-450x300.jpg`改为`lnmp.jpg`

我开始写的正则是这样：

```shell
sed 's/-[0-9]{3}x[0-9]{3}(.jpg|.png|.jpeg)/\1/g'
```

但sed需要的是这样，有的需要转义，有的不需要，真痛苦!

```shell
sed 's/-[0-9]\{3\}x[0-9]\{3\}\(.png\|.jpg\|.jpeg\)/\1/g'
```



## 主题定制

### 评论

静态博客的评论是个很大的问题，虽然也有很多可选的评论方案，但都有各自的缺点，打算先用用[valine](https://valine.js.org/), 这个基于js的评论插件是依赖于国内的`leancloud`服务商，免费使用有一些限制，以后会不会出现问题也不得而知，反正不是关键的点，先用用看。

开启`valine`在`NexT` v6里面很简单，直接修改主题的配置文件：

```yaml
valine:
  enable: true 
  appid:  填写从leancloud获取的appid 
  appkey: 填写从leancloud获取的appkey
```



有评论当然要有通知才好，找了找发现[valine有扩展通知的方案](http://www.zhaojun.im/hexo-valine-modify/)，用起来感觉还不错，使用了QQ 邮箱接收，微信会自动通知。

后面打算再加个`gitment`, 虽然应该很稳定，但需要用户登录`github`账号，也不是很完美，而且同时有两个评论看着也不是很爽。


{% note warning %} *文章写的不是很详细，细节部分请参考相关文档以及参考链接 * {% endnote %}


## TODO:

- [ ] 修改主题在首页文章下面加上tag， 类似于[hexo-theme-yelee](https://github.com/MOxFIVE/hexo-theme-yelee)主题一样
<hr />