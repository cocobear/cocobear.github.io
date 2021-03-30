title: 导出百度搜藏cang2html
tags:
  - bookmark
  - Python
id: 306
categories:
  - 编程相关
date: 2008-08-18 09:27:03
---

百度搜藏有不少问题，其中一个就是不能导出。

因为电脑丢了以前浏览器里的书签都没了，不过在百度测试搜藏的时候我用自己浏览器里的书签试过，刚好里面还有旧一点的书签，今天突然想起这事打算把把书签导出来，没想到竟然没有这个功能，今天没什么事就自己写了个工具，可以导出百度的搜藏，只需要手动确定用户名,程序里可以指定导出时使用的标签，因为没有登录，所以需要先把你所有的书签设置为公开：


	[cocobear@cocobear cang]$ cat cang.py 
	[cocobear@cocobear cang]$ cat cang.py 
	#!/usr/bin/env python
	#encoding=utf-8
	#Using GPL v2
	#Author: cocobear.cn@gmail.com

	import urllib2,urllib,cookielib,httplib
	import sys,re
	import gzip,StringIO

	user    = "cocobear_cn"         #user whose bookmarks you want get
	tag     = "from cang2html"      #tag that you want to mark
	id      = 0
	def cang2html(name,url,description):
	    global user
	    file_name = user+".html"
	    f = open(file_name,"a")
	    f.write("\t<dt>["+name+"](\)\n")
	    if description:
	        f.write("\t<dd>"+description+"</dd>\n")
	    f.close()

	def cang2adr(name,url,description):
	    global user,id
	    file_name = user+".adr"
	    f = open(file_name,"a")
	    f.write("#URL\n")
	    f.write("\tID="+str(id)+"\n")
	    f.write("NAME="+name+"\n\t"+"URL="+url+"\n")
	    if description:
	        f.write("\tDESCRIPTION="+description+"\n")
	    f.close()
	    id+=1

	def process_data(data):

	    result = re.findall("((?:http|ftp|https|file)://.*)\" target.*lnk\d+\">(.+?).*dc\d+\">(.*?)",data)
	    #print len(result)
	    for url,name,description in result:
	        #print name,url,description
	        cang2html(name,url,description)
	        cang2adr(name,url,description)

	def get_data(user,opener,page):
	    url = "http://cang.baidu.com/"+user+"/page/"+str(page)
	    gziped_data = opener.open(url).read()
	    gziped_stream = StringIO.StringIO(gziped_data)
	    data = gzip.GzipFile(fileobj=gziped_stream).read()
	    return data.decode('gbk').encode('utf-8')

	def init():
	    httplib.HTTPConnection.debuglevel  =  1 
	    cookie = cookielib.CookieJar()
	    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie))

	    exheaders = [("User-Agent","Opera/9.27 (X11; Linux x86_64; U; en)"),("Connection","Keep-Alive"),("Referer","http://cang.baidu.com"),("Accept","text/html, application/xml;q=0.9, application/xhtml+xml, */*;q=0.1"),("Accept-Charset","iso-8859-1, utf-8, utf-16, *;q=0.1"),("Cookie2","$Version=1"),("Accept-Encoding","deflate, gzip, x-gzip, identity, *;q=0"),]

	    opener.addheaders = exheaders
	    urllib2.install_opener(opener)
	    return opener

	def create_adr_file(name):
	    global tag,id

	    f = open(name,"w")
	    f.write("""Opera Hotlist version 2.0
	Options: encoding = utf8, version=3
	""")
	    f.write("#FOLDER\n")
	    f.write("\tID="+str(id)+"\n")
	    id+=1
	    f.write("\tNAME="+tag+"\n")
	    f.close()

	def create_html_file(name):
	    global tag

	    f = open(name,"w")
	    f.write("""< !DOCTYPE NETSCAPE-Bookmark-file-1>
	<meta HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
	<!--This is an automatically generated file.
	It will be read and overwritten.
	Code write by cocobear with Python
	Edit the file carefully!-->
	<title>Generated by cang2html</title>
	""")
	    f.write("

	# "+tag+"
	\n")
	    f.write("<dl>

	\n")
	    f.close()

	def end_html_file(name):

	    f = open(name,"a")
	    f.write("
	</dl>

	\n")
	    f.close()

	def main(argv=None):
	    global user,tag

	    opener=init()
	    data = get_data(user,opener,1)
	    match = re.search("（共(\d+)条）",data)
	    if match:
	        total = int(match.group(1))
	    else:
	        print "User has no bookmars!"
	        return 1
	    print "Total %d bookmarks" % total
	    print "Start getting ......"

	    create_html_file(user+".html")
	    create_adr_file(user+".adr")

	    process_data(data)
	    if total % 10 != 0:
	        for i in range(1,10):
	            if (total+i) % 10 == 0:
	                total+=i
	    total/=10

	    for i in range(2,total+1):
	        data = get_data(user,opener,i)
	        #print "process %d" % i
	        process_data(data)

	    end_html_file(user+".html")
	    #end_adr_file(user+".adr")

	    print "Success!\nLook %s.html for your bookmarks" % user
	    print "Success!\nLook %s.adr for your bookmarks" % user

	if __name__ == "__main__":
	    sys.exit(main())


该程序可以把百度搜藏导出为Opera使用的adr格式书签，以及html格式的书签，可以方便导入Opera浏览器，以及delicious(de.licio.us)等网上书签。

[代码下载](http://cocobear.github.io/code/cang.py)
?: 运算符表示“查找子模式的匹配字符串，但不包括反向引用中的匹配结果”
.*是贪婪匹配
.*?非贪婪匹配


## Comments

**[草儿](#4044 "2008-08-18 14:03:08"):** 好长。不过我收藏夹都是存在MP3随身携带的。

**[luguo](#4045 "2008-08-18 16:40:37"):** 恩，俺不用百度，因为这里没人知道百度这个东西。。。

**[Amankwah](#4046 "2008-08-18 21:26:01"):** ............ 我只依赖我的电脑~

**[youKnowMe](#5805 "2009-05-01 23:11:30"):** 博主我也需要一个windows下的，能给我一个吗？谢谢

**[我qq495979774](#4403 "2008-10-04 12:39:41"):** 想请教一下你写的代码如何导出百度搜藏呀

**[cocobear](#4616 "2008-11-18 11:34:24"):** 回楼上的，Python的很简单的，装个Python的解释器，然后在命令行下： python cang.py 就可以了，php好久没写了，

**[frankl](#4615 "2008-11-17 16:25:40"):** 兄弟能不能写个PHP的,Python不知道怎么用.

**[huan~!](#4925 "2009-01-27 12:01:00"):** 啊~~~LZ大大~~不会用呀~~！你能给俺编译个Windows下用的不？发到我的邮箱吧~！小弟万分感谢！

**[可可熊](#5825 "2009-05-04 09:48:02"):** http://www.box.net/shared/v73oa9s1tc 有点大，2M。

**[可可熊](#4329 "2008-09-18 09:03:10"):** 楼上的朋友可以留个联系方式，我可以帮你编译个Windows下的。

**[234](#4327 "2008-09-18 03:45:19"):** 大人……我很想导出我的搜藏，但是由于电脑小白，于是即使看到你的文章也不知道这个工具应该怎么使用Orz 还请大人闲暇之余具体相授啊！

**[可可熊](#4972 "2009-02-01 13:41:17"):** 你上面已经回答过了，很简单的，自己试一下。

**[可可熊](#4437 "2008-10-10 11:29:16"):** 回楼上的QQ俺不用，gtalk就是我的邮箱地址。

**[o仔](#6254 "2009-07-23 11:39:01"):** 这个windows版的下载不了。进入不了网址。。。 博主能否抽点时间发份到我的邮箱啊？

**[bymbym22](#6826 "2009-12-13 07:09:55"):** 先谢谢一下您把它弄成了WINDOWS下的程序。 我下载用了一下，就是有一个遗憾，百度搜藏里所有的链接都导出了，分类名不能导出。比如：我有六百多条搜藏，分成20个分类，这20个分类名称没有导出，而且导出后的HTML文件也没有分类，那么这样我就得重新进行20个分类才能用的。 不知道是不是我使用上的问题？ 如果不是，博主您能否把这个功能完善一下，好事做到底，谢谢。 我的邮箱是：bymbym22@sina.com

**[可可熊](#6829 "2009-12-13 11:42:52"):** 分类我确实没有导出 因为我用的时候就没做分类:-) 我抽空会看一下。

**[第十个黎明](#8992 "2010-11-25 17:25:00"):** 感谢^^!~
