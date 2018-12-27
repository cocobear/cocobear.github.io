title: CentOS配置LNMP环境
tags:
  - centos
  - Linux
  - MySQL
  - nginx
  - PHP
id: 1032
categories:
  - Linux
date: 2013-06-07 16:53:58
---

![Linux Nginx MySQL PHP](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/lnmp.jpg)

由于SELinux经常会使PHP FTP等常用的WEB服务产生一些奇怪的错误,所以我们在安装LNMP环境前先关闭SELinux:

     sed -i s/SELINUX=enforcing/SELINUX=disabled/ /etc/sysconfig/selinux

以下配置中, 所有编译安装的软件全部在/opt目录下, WEB服务器的目录为/home/www/html, MySQL数据目录为/opt/mysql/data

一. Nginx编译安装

安装编译环境以及依赖包:

     yum install wget> 
     yum install pcre-devel> 
     yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel openldap-clients openldap-servers make gd gd-devel

添加WEB服务器专用的用户:

     /usr/sbin/groupadd www> 
     /usr/sbin/useradd -g www www

编译安装Nginx:

     wget http://nginx.org/download/nginx-1.4.1.tar.gz> 
     mkdir /opt/nginx> 
     
     tar zxvf nginx-1.4.1.tar.gz> 
     cd nginx-1.4.1> 
     ./configure --user=www --group=www --prefix=/opt/nginx --with-http_ssl_module --with-pcre --with-http_image_filter_module> 
     
     make && make install

创建Nginx服务器目录:

     mkdir /home/www/html

编辑nginx.conf文件:


	[root@HJY-SERVER conf]# sed '/#/d' /opt/nginx/conf/nginx.conf | sed '/^$/d'
	user  www www;
	worker_processes  1;
	events {
	    worker_connections  1024;
	}
	http {
	    include       mime.types;
	    default_type  application/octet-stream;
	    sendfile        on;
	    keepalive_timeout  65;
	    gzip  on;
	    server {
	        listen       80;
	        server_name  localhost;
	        location / {
	            root   /home/www/html;
	            index  index.php index.html index.htm;
	        }
	    }
	}



配置Nginx自启动, Nginx官方有rpm包格式的二进制安装包,下载一个从里面提取nginx启动脚本,这里可以借用一下:

     wget http://nginx.org/packages/centos/6/x86_64/RPMS/nginx-1.4.1-1.el6.ngx.x86_64.rpm> 
     rpm2cpio nginx-1.4.1-1.el6.ngx.x86_64.rpm | cpio -idmv

nginx脚本: 
	#!/bin/sh
	# nginx        Startup script for nginx
	#
	# chkconfig: - 85 15
	# processname: nginx
	# config: /etc/nginx/nginx.conf
	# config: /etc/sysconfig/nginx
	# pidfile: /var/run/nginx.pid
	# description: nginx is an HTTP and reverse proxy server
	#
	### BEGIN INIT INFO
	# Provides: nginx
	# Required-Start: $local_fs $remote_fs $network
	# Required-Stop: $local_fs $remote_fs $network
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: start and stop nginx
	### END INIT INFO

	# Source function library.
	. /etc/rc.d/init.d/functions

	NGINX=/opt/nginx/sbin/nginx
	CONFFILE=/opt/nginx/conf/nginx.conf

	prog=nginx
	nginx=${NGINX}
	conffile=${CONFFILE}
	lockfile=${LOCKFILE-/opt/nginx/nginx.lck}
	pidfile=${PIDFILE-/opt/nginx/logs/nginx.pid}
	SLEEPMSEC=100000
	RETVAL=0

	start() {
	    echo -n $"Starting $prog: "

	    daemon --pidfile=${pidfile} ${nginx} -c ${conffile}
	    RETVAL=$?
	    echo
	    [ $RETVAL = 0 ] &amp;&amp; touch ${lockfile}
	    return $RETVAL
	}

	stop() {
	    echo -n $"Stopping $prog: "
	    killproc -p ${pidfile} ${prog}
	    RETVAL=$?
	    echo
	    [ $RETVAL = 0 ] &amp;&amp; rm -f ${lockfile} ${pidfile}
	}

	reload() {
	    echo -n $"Reloading $prog: "
	    killproc -p ${pidfile} ${prog} -HUP
	    RETVAL=$?
	    echo
	}

	upgrade() {
	    oldbinpidfile=${pidfile}.oldbin

	    configtest -q || return 6
	    echo -n $"Staring new master $prog: "
	    killproc -p ${pidfile} ${prog} -USR2
	    RETVAL=$?
	    echo
	    /bin/usleep $SLEEPMSEC
	    if [ -f ${oldbinpidfile} -a -f ${pidfile} ]; then
	        echo -n $"Graceful shutdown of old $prog: "
	        killproc -p ${oldbinpidfile} ${prog} -QUIT
	        RETVAL=$?
	        echo
	    else
	        echo $"Upgrade failed!"
	        return 1
	    fi
	}

	configtest() {
	    if [ "$#" -ne 0 ] ; then
	        case "$1" in
	            -q)
	                FLAG=$1
	                ;;
	            *)
	                ;;
	        esac
	        shift
	    fi
	    ${nginx} -t -c ${conffile} $FLAG
	    RETVAL=$?
	    return $RETVAL
	}

	rh_status() {
	    status -p ${pidfile} ${nginx}
	}

	# See how we were called.
	case "$1" in
	    start)
	        rh_status &gt;/dev/null 2&gt;&amp;1 &amp;&amp; exit 0
	        start
	        ;;
	    stop)
	        stop
	        ;;
	    status)
	        rh_status
	        RETVAL=$?
	        ;;
	    restart)
	        configtest -q || exit $RETVAL
	        stop
	        start
	        ;;
	    upgrade)
	        upgrade
	        ;;
	    condrestart|try-restart)
	        if rh_status &gt;/dev/null 2&gt;&amp;1; then
	            stop
	            start
	        fi
	        ;;
	    force-reload|reload)
	        reload
	        ;;
	    configtest)
	        configtest
	        ;;
	    *)
	        echo $"Usage: $prog {start|stop|restart|condrestart|try-restart|force-re                                                                             load|upgrade|reload|status|help|configtest}"
	        RETVAL=2
	esac

	exit $RETVAL


复制nginx脚本到init.d启动脚本目录下:

     cp ./etc/rc.d/init.d/nginx /etc/rc.d/init.d/> 
     chmod +x nginx > 
     chkconfig --add nginx> 
     chkconfig --level 345 nginx on

测试配置并启动:

     service nginx configtest> 
     service nginx start

到现在Nginx安装已经完成.

二. MySQL编译安装

安装依赖环境:

     yum install cmake bison

下载稳定版本的MySQL 5.6:

     wget http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.12.tar.gz

添加mysql用户:

     groupadd mysql> 
     useradd -r -g mysql mysql

编译安装:

     cd mysql-5.6.12> 
     
     cmake -DCMAKE_INSTALL_PREFIX=/opt/mysql -DMYSQL_UNIX_ADDR=/tmp/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS:STRING=utf8,gbk -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DENABLED_LOCAL_INFILE=1 -DMYSQL_DATADIR=/opt/mysql/data -DINSTALL_MYSQLTESTDIR=> 
     
     make && make install

编译安装完成后进行配置:

     chmod +w /opt/mysql/> 
     chown -R mysql:mysql /opt/mysql/> 
     <del datetime="2013-06-08T07:46:57+00:00">ln -s /usr/local/mysql/lib/libmysqlclient.so.18 /usr/lib/libmysqlclient.so.18</del>> 
     ln -s /opt/mysql/lib/libmysqlclient.so.18 /usr/lib64/libmysqlclient.so.18> 
     cd /opt/mysql/support-files/> 
     cp mysql.server /etc/rc.d/init.d/mysqld> 
     chmod +x /etc/init.d/mysqld> 
     
     ./scripts/mysql_install_db --defaults-file=/opt/mysql/my.cnf --basedir=/opt/mysql/ --datadir=/opt/mysql/data/ --user=mysqlshell> bin/mysqld_safe --user=mysql > 
     
     vi /etc/init.d/mysqld（编辑此文件，查找并修改以下变量内容：）> 
     basedir=/opt/mysql> 
     datadir=/opt/mysql/data> 
     
     chkconfig --add mysqld> 
     chkconfig --level 345 mysqld on> 
     
     service mysqld start> 
     
     /usr/local/mysql/bin/mysqladmin password [new-password]

三. 编译安装PHP

依赖的软件包:

     wget "http://downloads.sourceforge.net/mhash/mhash-0.9.9.9.tar.gz?modtime=1175740843&big_mirror=0"> 
     wget "http://downloads.sourceforge.net/mcrypt/libmcrypt-2.5.8.tar.gz?modtime=1171868460&big_mirror=0"> 
     wget "http://downloads.sourceforge.net/mcrypt/mcrypt-2.6.8.tar.gz?modtime=1194463373&big_mirror=0"> 
     
     tar zxvf libmcrypt-2.5.8.tar.gz> 
     cd libmcrypt-2.5.8/> 
     ./configure --prefix=/opt/libs> 
     make> 
     make install> 
     cd libltdl/> 
     ./configure --prefix=/opt/libs --enable-ltdl-install> 
     make> 
     make install> 
     cd ../../> 
     
     tar zxvf mhash-0.9.9.9.tar.gz> 
     cd mhash-0.9.9.9/> 
     ./configure --prefix=/opt/libs> 
     make> 
     make install> 
     cd ../

把安装好的libs目录加入系统路径中:

     cd /etc/ld.so.conf.d/> 
     [root@HJY-SERVER ld.so.conf.d]# cat opt-libs.conf> 
     /opt/libs/lib

编译安装PHP：

	wget http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.12.tar.gz
	tar zxvf mysql-5.6.12.tar.gz
	cd mysql-5.6.12.tar.gz

	export LIBS="-lm -ltermcap -lresolv"
	export DYLD_LIBRARY_PATH="/opt/mysql/lib/:/lib/:/usr/lib/:/usr/local/lib:/lib64/:/usr/lib64/:/usr/local/lib64"

	export LD_LIBRARY_PATH="/opt/mysql/lib/:/lib/:/usr/lib/:/usr/local/lib:/lib64/:/usr/lib64/:/usr/local/lib64"

	./configure --prefix=/opt/php --with-config-file-path=/opt/php/etc --with-mysql=/opt/mysql --with-mysqli=/opt/mysql/bin/mysql_config --with-iconv-dir --with-freetype-dir=/opt/libs --with-jpeg-dir=/opt/libs --with-png-dir=/opt/libs --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-fpm --enable-mbstring --with-mcrypt=/opt/libs --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap 

	make
	make install

配置：

     cp php.ini-development /opt/php/etc/php.ini> 
     cd ../> 
     
     mv /opt/php/etc/php-fpm.conf.default /opt/php/etc/php-fpm.conf

Update 2013.6.8:

安装好后没有对PHP进行测试，今天测试了一下，结果PHP脚本不能被执行，默认在浏览器里被下载了下来，习惯了以前用Apache不用对PHP进行配置, 现在要继续未完成的配置：

     cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm> 
     chkconfig --add php-fpm> 
     chkconfig --level 345 php-fpm on

修改php-fpm文件：

     pid = run/php-fpm.pid> 
     user = www> 
     group = www

修改nginx.conf文件，在server字段处添加以下内容：

     location ~ .*\.(php|php5)?$> 
                 {> 
     
                     root   /home/www/html;> 
                     fastcgi_pass 127.0.0.1:9000;> 
                     fastcgi_index index.php;> 
                     include fastcgi.conf;> 
                 }

配置完成后重起服务：

     service nginx restart> 
     service php-fpm restart

可以用下面的命令查看占用端口的进程名，pid，nginx和php-fpm分别占用80和9000端口:

     netstat -anpo ｜ grep tcp

参考文章:
http://blog.s135.com/nginx_php_v7/
http://www.centos.bz/2011/09/linux-compile-install-mysql-5-5-15-from-source/
http://www.boluo.org/archives/chkconfig-nginx-on.html
http://www.linuxsong.org/2010/09/cpio/

btw: wordpress中想要添加空行还不容易,p,br 标签都会被过滤掉, 找到一个简单的办法适用于最新的3.5.1版本，安装TinyMCE Advanced插件， 打开插件里面的选项，禁止自动删除< p >, < br \ >标签，不过我这里测试的只支持： < br \ > 标签.
## Comments

**[Amankwah](#25355 "2013-06-08 11:48:18"):** 最近你在干嘛呢？？

**[可可熊](#25358 "2013-06-08 12:56:08"):** 配个服务器么！ 好奇怪吗？

