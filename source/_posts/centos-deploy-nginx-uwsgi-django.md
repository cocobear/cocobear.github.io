title: CentOS部署 Nginx+uWSGI+Django
tags:
  - centos
  - Django
  - Linux
  - nginx
  - Python
  - uwsgi
  - virtualenv
id: 1138
categories:
  - Linux
date: 2013-06-19 14:53:33
---

[![django-nginx-uwsgi](http://7sbxmt.com1.z0.glb.clouddn.com/django-nginx-uwsgi-610x284.jpg)](http://c.kensou.me/blog/centos-deploy-nginx-uwsgi-django/django-nginx-uwsgi/)

创建单独的django环境：

     virtualenv django

激活django环境：

     [root@HJY-SERVER html]# source ../venv/django/bin/activate

新建一个工程：
	(django)[root@HJY-SERVER html]# django-admin.py startproject mcard
	(django)[root@HJY-SERVER html]# tree mcard/
	mcard
	├── manage.py
	└── mcard> 
	 ├── __init__.py
	 ├── settings.py
	 ├── urls.py
	 └── wsgi.py
	1 directory, 5 files

最外面这个目录mcard可以任意修改，只要保证nginx指向这个目录就可以了，下一层目录中的mcard存放的是该工程的核心配置文件，settings.py为数据库、语言、链接库等配置，urls.py指定了转发规则，wsgi.py则是入口程序，需要在uwsgi的配置文件中指定该文件，下面我们配置uwsgi：

	(django)[root@HJY-SERVER html]# cat /opt/uwsgi/conf/mcard.ini
	[uwsgi]
	#variables
	base=/opt/uwsgi
	#config
	socket=%(base)/run/%n.sock
	daemonize=%(base)/run/%n.log
	pidfile=%(base)/run/%n.pid
	gid=www
	uid=www
	virtualenv=/home/www/venv/django
	chdir=/home/www/html/%n
	pathonpath=/home/www/html/%n
	module=%n.wsgi:application
	master=True
	processes=4
	max-requests=5000
这次对uwsgi的配置脚本进行了优化，考虑了多个app共存的情况，使用%n来代替app的名称，而这个%n就是这个配置的文件名，如mcard.ini, uwsg解析这个配置文件的时候会自动把%n替换为mcard,这样如果要新加一个app，只需要

     copy mcard.ini mmq.ini> 
     (django)[root@HJY-SERVER html]# ls /opt/uwsgi/conf/> 
     mcard.ini  mmq.ini  mobile.ini

就完成了，不需要再手动改配置文件里面的应用名称了，只需要再创建一个应用目录就完成应用的增加。

而启动的时候修改一下启动脚本，使用emperor 模式来运行uwsgi， 这样 有多个应用（比如一个bottle应用，两个django应用）你就不需要写三个自启动的脚本，只需要把原来启动脚本中的启动参数改类似下面的模式：

     uwsgi --emperor "/var/www/*/conf/uwsgi.ini"

下面是截取的 /etc/init.d/uwsgi脚本片段：

	. /etc/rc.d/init.d/functions

	UWSGI=/opt/uwsgi/uwsgi
	CONFFILE=/opt/uwsgi/conf

	prog=uwsgi
	app=mmq
	uwsgi=${UWSGI}
	conffile=${CONFFILE}
	lockfile=/opt/uwsgi/run/${prog}.lck
	pidfile=/opt/uwsgi/run/${prog}.pid
	logfile=/opt/uwsgi/run/${prog}.log
	SLEEPMSEC=100000
	RETVAL=0

	start() {
	    echo -n $"Starting $prog: "

	    daemon ${uwsgi} --emperor ${conffile} -d ${logfile} --pidfile=${pidfile}
	    RETVAL=$?
	    echo
	    [ $RETVAL = 0 ] &amp;&amp; touch ${lockfile}
	    return $RETVAL
	}


接下来更新一下nginx的配置文件，以下配置支持应用的情况为：

一个PHP应用在80端口；
一个bottle应用配置在8051端口；
两个django应用配置在8052端口，分别使用/mcard和/mmq访问；


    server {
        listen       8051;
        server_name  localhost;
        root   /home/www/html/mobile;
        location / {
            include uwsgi_params;
            uwsgi_pass unix:///opt/uwsgi/run/mobile.sock;
        }
    }

    server {
        listen       8052;
        server_name  localhost;
        #下面这行指定了访问/mmq转发到mmq.sock这个uwsgi进程上
        location /mmq/ {
            include uwsgi_params;
            uwsgi_pass unix:///opt/uwsgi/run/mmq.sock;
        }
        #下面指定了mmq应用静态目录的位置，因为该应用基于admin控制台，所以把静态文件指向django系统默认的目录中
        location /mmq/static/ {
            alias /home/www/venv/django/lib64/python2.6/site-packages/django/contrib/admin/static/;
        }
        #访问/mcard/转发到mcard.sock这个uwsgi进程上
        location /mcard/ {
            include uwsgi_params;
            uwsgi_pass unix:///opt/uwsgi/run/mcard.sock;
        }
    }
    server {
        listen       80;
        server_name  localhost;
        root   /home/www/html;
        location ~ .*\.(php|php5)?$
        {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            include fastcgi.conf;
        }
    }


Web文档的目录结构为：
	(django)[root@HJY-SERVER html]# ls
	index.php  mcard/  mmq/  mobile/

配置好后重起系统，分别访问
http://127.0.0.1:8051
http://127.0.0.1:8052/mcard/
http://127.0.0.1:8052/mmq/

参考文章：
第二、三篇文章介绍的很详细，包括部署应用的目录结构，配置文件的写法，都挺值得参考的。

http://obmem.info/?p=703
http://www.collabspot.com/2012/08/14/setting-up-nginx-uwsgi-python-ubuntu-12-04/
http://jawher.me/2012/03/16/multiple-python-apps-with-nginx-uwsgi-emperor-upstart/
http://loosky.net/archives/2665.html
http://blog.csdn.net/quicktest/article/details/7818781
