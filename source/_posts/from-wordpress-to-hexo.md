title: 将博客从WordPress迁移至GitHub(GitCafe)+Hexo
date: 2014-12-26 15:18:20
tags: [hexo, git, github, wordpress]
categories: 互联网
toc: true
comments: true

---

题记：对`markdown`不了解的同学可以先观看[这段slide](http://xiaocong.github.io/slides/writing-documentation-with-markdown/#/markdown-index)

　　好长一段时间内都没有什么热情去写博客，有的时候需要记录一点东西也直接打开`notepad2`，用[markdown]格式写完后保存在了Dropbox里，因为是随便记录，所以基本上没什么格式，一般都是一大堆的项目符号`* xxx *xxx`。不记得是哪天看到别人使用`markdown+github`来写博客，界面简洁清爽，顿时觉得这种方式才是最适合自己的博客模式，不用登录后台，不用关心托管的服务器，直接用自己喜欢的编辑器写博客，然后`git push`, 一篇博客就诞生了。甚至你可以通过Git来查看你对文章进行的所有修改，一切都显得那么合适，这才是最有情怀的blog方式。

　　使用`markdown+github`写博客的方案一般有`jekyll, Octopress, hexo` 经过比较后，最后我选择了`hexo`, 一个基于`nodejs`的博客，我主要看中了他的部署简单，速度快的特点。接下来就是从wordpress迁移过来了。
　　
##从WordPress迁移数据

### 数据导出

1. 通过wordpress后台的export导出xml格式的数据
    如果原来的域名已经失效，需要更换新的域名时，可以通过修改wp-config.php文件，添加下面两行来实现更换域名:
        
        define('WP_HOME','http://new_domain/blog/');
        define('WP_SITEURL','http://new_domain/blog/');
        
2. 从服务商主机中下载wordpress目录下的uploads中的所有数据(主要是写文章时插入的图片)


###安装hexo
1. 安装Node.js (Windows下有可执行的安装包)
		sudo apt-get install nodejs
	如果出现找不到node命令的时候，需要再安装`nodejs-legacy`, 其实就是一个`ln -s node nodejs`

2. 安装hexo
		npm install -g hexo
		mkdir hexo
		cd hexo
		hexo init
		npm install
		hexo generate
		hexo server
		
###xml数据导入

* ####使用hexo提供的插件来导入
    ```bash
	npm install hexo-migrator-wordpress --save
	hexo migrate wordpress wordpress.2014-12-18.xml
	```
　　因为原始的wordpress文章中有html标签，通过hexo-migrator-wordpress插件生成的markdown文件中仍然包含了html标签，所以转换后的页面会比较乱，仍然需要把xml中的html标签转化为markdown语法，这样才能使得hexo生成html页面和wordpress处理后一样的效果。

* ####使用第三方工具转换
　　通过能过万能的Gooogle，我找到了一个叫[wp2md](https://github.com/verygamer/wp2md)的工具，专门是做上面这件事的。不过使用官方建议的安装方式时出错了，最后我手工从github中下载了代码，然后pip install依赖的包markdown,手动下载了另一个依赖包html2text.py。
　　最后使用python wp2md.py -d ./ wordpress-dump.xml 来生成markdown文件，第一次生成的时候报错了，想到应该是xml文件格式的问题，于是使用xmllint检查一下wordpress导出的xml文件
```bash
xmllint --noout  wordpress.2014-12-18.xml
```
　　根据错误提示修改了xml文件，再次使用wp2md.py，成功了：
```bash
Dumping index to 'C:\wp2md-master\index.md'

Total: posts: 364; pages: 3; comments: 2124
Elapsed time: 1.998 s
```
　　但是处理后的结果hexo解析时会有问题，不能完美显示，其中tags和categories都没有，而且更严重的问题是处理后的目录结构是以年为目录，直接把这些文件夹拷到hexo的source/_posts/目录下后hexo server无法启动。所以还是需要使用hexo自带的插件处理，然后再想办法掉去其中的HTML标签比较靠谱。wp2md里面有用到html2text.py这个工具，可以把html代码转换为markdown格式，本来想用html2text.py把hexo生成的所有markdown文件全处理一遍。但是hexo生成的文件已经是markdown格式了，继续用html2text.py处理的时候会把markdown格式破坏。

* ####最后还是手动处理
　　想不到什么别的好办法了，索性直接手动处理了，300多篇文章，使用Beyond Compare比较hexo和wp2md生成的md文件，然后手动修改hexo生成的文章，处理了几个小时，终于完工了，可能会有不小心露掉的，不碍大事，就先这样吧。

###评论迁移
　　文章内容没什么问题了，接下来就是评论的问题，以前的博客有2000多的评论，而hexo转换出来的是不支持评论的，似乎disqus可以做评论迁移，但是由于我改过一些文章的标题，而且disqus在国内使用和访问都不是很方便，所以决定把wp2md生成的评论直接添加到过去文章的内容里。写了一个Python脚本来完成这个任务，几行代码就可以搞定。

###图片迁移
　　接下来就是迁移以前博客里面的图片了，看了下wordpress里面的图库，发现没有多少图片，直接放在hexo的source目录下面也是可以的，不过为了让hexo更简洁一些，还是把图片放在别的地方，看到好多用hexo的用户都推荐使用七牛这个图床，我也试了一下，但是觉得不太好用，官方提供的Linux下的同步工具老是失败，而且提供的外链域名是个不能修改的域名(如果要个性化域名好像是要实名认证的:-( 。最后直接用web页面上传了以前的图片，然后替换了markdown文件中的链接。先这样用一用吧，如果有更好的图床，再折腾吧。
```
sed -e 's/c.kensou.me\/blog\/wp-content\/uploads\/[0-9]\{4\}\/[0-9]\{2\}/7sbxmt.com1.z0.glb.clouddn.com/'
```

##hexo优化
###安装主题
```bash
git clone https://github.com/wzpan/hexo-theme-freemind.git themes/freemind
```
然后修改`_config.yml` , 修改内容：`theme: freemind`

###加快访问速度
可以把主题中使用的js等库替换为公共CDN提供的地址，例如：
http://libs.baidu.com/jquery/2.0.3/jquery.min.js


###评论：
可以使用Disqus和多数。Disqus是hexo默认支持的，直接在配置文件中添加你的用户名就可以，而多数是freemind这个主题支持的，也是直接在配置文件中添加：
```
# Disqus
disqus_shortname:

#duoshuo
duoshuo_shortname: cocobear
```

### 部署到github和gitcafe
试了几种_config.yml的配置方式，我使用的hexo(2.8.3)版本只有下面这种方式是两个服务器都可以成功push的:
```yaml
deploy:
    type: github
    repo: https://github.com/cocobear/cocobear.github.io.git
    type: github
    repo: https://gitcafe.com/cocobear/cocobear.git
    branch: gitcafe-pages
```
配置好后就可以使用下面的命令进行发布了：
```
hexo clean
rm -rf .deploy  
hexo g
hexo d
```

##后记
目前这样的博客写作方式比较符合Unix的哲学，

> 程序应该只关注一个目标，并尽可能把它做好。让程序能够互相协同工作。应该让程序处理文本数据流，因为这是一个通用的接口。

其实互联网的发展趋势也是朝着精细化服务的方向

>	Every Unix Command eventually become an internet service. 每条 Unix/Linux 命令最终将成为一种互联网服务
	Grep -> Google
	rsync - Dropbox
	man -> Stack Overflow
	cron -> ifttt
	来自推友@cdixon。
	
[markdown]: http://daringfireball.net/projects/markdown/syntax
参考链接:
http://gfrog.net/2013/03/convert-wordpress-to-octopress/
http://www.aogun.com/2013/04/10/wordpress-2-markdown/
http://bubbyroom.com/2013/08/11/migrate-my-blog-from-wordpress-to-hexo/
http://armsword.com/2014/11/22/move-from-wordpress-to-hexo/
http://ibruce.info/2013/11/22/hexo-your-blog/
http://blog.csdn.net/qmhball/article/details/8955588<!--se_discussion_list:{"nBtluLREDbgH45R6jLT3OrQw":{"selectionStart":61,"type":"conflict","selectionEnd":69,"discussionIndex":"nBtluLREDbgH45R6jLT3OrQw"},"kWdsG23zzTzAqQyvwE59DzTW":{"selectionStart":96,"type":"conflict","selectionEnd":276,"discussionIndex":"kWdsG23zzTzAqQyvwE59DzTW"},"f6PleA9BZtUVcxe9roeRBuFS":{"selectionStart":508,"type":"conflict","selectionEnd":568,"discussionIndex":"f6PleA9BZtUVcxe9roeRBuFS"},"BbRyDtZNnrALXwn8jg40WN2N":{"selectionStart":645,"type":"conflict","selectionEnd":1054,"discussionIndex":"BbRyDtZNnrALXwn8jg40WN2N"},"nNExzvumCgIWnPQUV6WuktBJ":{"selectionStart":1078,"type":"conflict","selectionEnd":1084,"discussionIndex":"nNExzvumCgIWnPQUV6WuktBJ"},"m3dlFYj8oSy1GrNwwJnAIieG":{"selectionStart":1143,"type":"conflict","selectionEnd":1310,"discussionIndex":"m3dlFYj8oSy1GrNwwJnAIieG"},"nKMPgBneoA6jFxoviw7kP7fY":{"selectionStart":1348,"type":"conflict","selectionEnd":1354,"discussionIndex":"nKMPgBneoA6jFxoviw7kP7fY"},"B5C5B7SXU0xJgTlOcorrGJxv":{"selectionStart":1379,"type":"conflict","selectionEnd":1586,"discussionIndex":"B5C5B7SXU0xJgTlOcorrGJxv"},"flunmdRiqsxgyHNdFuF7Xpjm":{"selectionStart":1712,"type":"conflict","selectionEnd":1757,"discussionIndex":"flunmdRiqsxgyHNdFuF7Xpjm"},"F2QkK5KArOTdOv25PcrJLXYM":{"selectionStart":1804,"type":"conflict","selectionEnd":2020,"discussionIndex":"F2QkK5KArOTdOv25PcrJLXYM"},"vhx3fO6McHdSDi93uuNACCdq":{"selectionStart":2060,"type":"conflict","selectionEnd":2064,"discussionIndex":"vhx3fO6McHdSDi93uuNACCdq"},"KifyzvzqztB2RSeRlaAZJas5":{"selectionStart":2085,"type":"conflict","selectionEnd":2090,"discussionIndex":"KifyzvzqztB2RSeRlaAZJas5"},"D156KPx2gkU4f64i21xgotvY":{"selectionStart":2203,"type":"conflict","selectionEnd":2425,"discussionIndex":"D156KPx2gkU4f64i21xgotvY"},"Txw3SjaSt6TimIzYzTC7ajiz":{"selectionStart":2467,"type":"conflict","selectionEnd":2735,"discussionIndex":"Txw3SjaSt6TimIzYzTC7ajiz"},"GHba2sVihSwcVUI3dXsSbTyL":{"selectionStart":2936,"type":"conflict","selectionEnd":3138,"discussionIndex":"GHba2sVihSwcVUI3dXsSbTyL"},"T8lZNbRP36eXV7H9H72wiRcV":{"selectionStart":3198,"type":"conflict","selectionEnd":3985,"discussionIndex":"T8lZNbRP36eXV7H9H72wiRcV"},"XFLHKv71I3RGGda7sqfiLdhk":{"selectionStart":4060,"type":"conflict","selectionEnd":4519,"discussionIndex":"XFLHKv71I3RGGda7sqfiLdhk"}}-->