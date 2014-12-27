title: 使用Django Admin做一个简单的应用
tags:
  - Django
  - Python
id: 872
categories:
  - Life
date: 2011-03-16 14:29:44
---

使用Django 的admin做一个简单的应用：

一、安装WSGI
官方推荐使用WSGI模式，而不是mod_python方式在Apache上运行Django。拿Fedora为例，直接:

     yum install mod_wsgi 

就可以完成WSGI的安装

二、配置Apache

安装好mod_wsgi后，Apache的服务器配置里面会自动加入mod_wsgi的调用，所以只需要配置Django相关的部分：

     #add for mod_wsig> 
     Alias /media "/usr/lib/python2.5/site-packages/django/contrib/admin/media/"> 
         <directory "/usr/lib/python2.5/site-packages/django/contrib/admin/media/">> 
           AllowOverride All> 
           Options None> 
           Order allow,deny> 
           Allow from all> 
         </directory>> 
     
     WSGIScriptAlias /mmq /var/www/Django/mmq/apache/django.wsgi

这里我们假定项目名为mmq，位置在/var/www/Django/mmq，服务器访问的根目录为/mmq(比如你访问这个应用需要使用地址:http://127.0.0.1/mmq/mmq/)

Alias /media是指定了Django访问静态文件的位置，因为我这里用到Django admin，所以需要把它指向Django admin静态文件的位置；当然你自己项目里面的模版文件会自动覆盖上面的设置。

如你可以mmq\templates\admin在这个位置创建一个index.html文件来自定义自己的admin主界面。

三、配置Django项目 
刚才我们在http.conf文件中指定了一个django运行的根脚本，现在我们需要创建这个脚本：

django.wsgi:
     import os> 
     import sys> 
     
     sys.path.append('/var/www/Django')> 
     os.environ['DJANGO_SETTINGS_MODULE'] = 'mmq.settings'> 
     
     import django.core.handlers.wsgi> 
     application = django.core.handlers.wsgi.WSGIHandler()

urls.py配置：
     from django.conf.urls.defaults import *> 
     from django.contrib import admin> 
     admin.autodiscover()> 
     
     from mmq.views import *> 
     
     urlpatterns = patterns('',> 
         (r'^mmq/', include(admin.site.urls)),> 
         (r'^mmq/admin/', include(admin.site.urls)),> 
     )
这里把首地址直接指向了admin的登录页面。

settings.py配置：

     DATABASE_NAME = '/var/www/Django/mmq/mmq.db' > 
     MEDIA_ROOT = '/var/www/Django/mmq/'> 
     
     ADMIN_MEDIA_PREFIX = '/media/'> 
     TEMPLATE_LOADERS = (> 
         'django.template.loaders.filesystem.load_template_source',> 
         'django.template.loaders.app_directories.load_template_source',> 
     #     'django.template.loaders.eggs.load_template_source',> 
     )> 
     
     MIDDLEWARE_CLASSES = (> 
         'django.middleware.common.CommonMiddleware',> 
         'django.contrib.sessions.middleware.SessionMiddleware',> 
         'django.contrib.auth.middleware.AuthenticationMiddleware',> 
     )> 
     
     ROOT_URLCONF = 'mmq.urls'> 
     
     TEMPLATE_DIRS = (> 
         '/var/www/Django/mmq/templates/',> 
     )> 
     
     INSTALLED_APPS = (> 
         'django.contrib.auth',> 
         'django.contrib.sessions',> 
         'django.contrib.contenttypes',> 
         # Uncomment the next line to enable the admin:> 
         'django.contrib.admin',> 
         'mmq',> 
     )
在Linux上使用sqlite需要指定绝对路径，而且需要给Apache数据库所在父母录的写权限.

整合Django和WSGI有些麻烦，更详细的内容可以参考WSGI官方的文档:
[http://code.google.com/p/modwsgi/wiki/IntegrationWithDjango](http://code.google.com/p/modwsgi/wiki/IntegrationWithDjango)

关于使用Django admin 定制自己的应用在Django官方的文档中讲了不少，这里就不具体描述了。

想更改admin的CSS不过，不能像template里的文件一样，直接在app中写个来覆盖，路过的哪位知道的话麻烦留个言。

Django的设计、使用感觉挺不错的，可惜布署很是麻烦，下个别人写的app半天都运行不起来，自己写个app用Django自带的服务器运行没问题，可是移到Apache上又是一堆问题，如果能象PHP一样简单的布署那么Django应该会有更好的发展。
## Comments

**[cesc](#12455 "2012-01-16 18:05:54"):** 您好，我非常喜欢您的博客，不管是内容还是UI，请问您是怎么做的，盼复

**[ben](#9896 "2011-04-21 19:04:45"):** 修改admin的CSS,你在Apache静态文件里做的有链接， Alias /media “/usr/lib/python2.5/site-packages/django/contrib/admin/media/” 把/usr/lib/python2.5/site-packages/django/contrib/admin/media/里复制一份，随便改吧 为什么要改CSS那?俺觉得怪好看的呀~

**[cocobear](#12517 "2012-01-21 11:01:03"):** 博客的程序用的是WordPress，主题用的是：gplus，你可以在网上下载到。

