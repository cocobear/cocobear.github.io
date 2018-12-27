title: Django记录一
tags:
  - Django
  - Python
id: 1166
categories:
  - Life
date: 2013-06-26 14:01:59
---

![django](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/django.png)

* 如何清空app对应的数据库表项并重建数据库表 


     python manage.py sqlclear mcard | python manage.py dbshell> 
     python manage.py syncdb

* 如何使用South管理Django数据库（model)：
<!--more-->

使用Django做开发的，有时候一个app运行了一段时间，有新的需求要在数据库表项增加一个字段，这时候South就可以帮到大忙了。

首先安装south, 可以使用pip south来安装，然后把South添加到项目的INSTALLED_APPS 中，运行 ./manage.py syncdb 加载South 用到的数据库表项。安装好South后需要把现有的app添加到South里，可以使用下面的命令：

     ./manage.py schemamigration mmq --initial> 
     ./manage.py migrate mmq 0001 --fake

如果对models进行了修改后，则可以运行下面的命令来修改数据库：

     ./manage.py schemamigration app_name--auto > 
     ./manage.py migrate app_name

如果你的操作失败了（如你执行了 migrate app_name命令，但是由于新加的field是非空，不能正常更新数据库），但South还是把你的操作记录进了数据库，你可以手动回退操作：

     rm app_name/migrations/000X_something_you_have_done.py> 
     ./manage.py dbshell > 
     sqlite> select * from south_migrationhistory;> 
     sqlite> delete from south_migrationhistory where id=xx;> 
     sqlite> drop table _south_new_table_name;

* 使用自定义的locale文件：

首先要在项目的settings.py文件中添加locale目录位置，如果你的locale位于你的app下，则可以略过下面的步骤：

     LOCALE_PATHS = ( os.path.join(BASE_DIR , 'locale'), ) 


请注意上面的LOCALE_PATHS 是一个tuple,如果你只有一个位置，也需要在后面加一个逗号，否则Django不会报任何错误，但是它不会使用这个位置的locale文件。

配置好locale目录后，在项目目录下运行命令：

     ./manage.py makemessages -l zh_CN

将生成如下的目录结构：
     (django)[www@HJY-SERVER manage]$ tree locale/> 
     locale/> 
     └── zh_CN> 
         └── LC_MESSAGES> 
             ├── django.mo> 
             └── django.po

如果使用Django admin开发应用，[grappelli](https://github.com/sehmaschine/django-grappelli)和[adminactions](https://github.com/saxix/django-adminactions)是两个不错的插件，安装很简单，请参考官方的文档说明。

参考文章：

http://agiliq.com/blog/2012/01/south/
http://simple-is-better.com/news/798
http://south.readthedocs.org/en/latest/tutorial/part3.html
