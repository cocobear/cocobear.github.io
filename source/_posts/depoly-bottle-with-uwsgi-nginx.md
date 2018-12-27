title: CentOS部署 Nginx+uWSGI+Bottle
tags:
  - bottle
  - centos
  - nginx
  - Python
  - uwsgi
  - virtualenv
id: 1107
categories:
  - Linux
date: 2013-06-08 17:53:25
---

![bottle_uwsgi](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/bottle_uwsgi.jpg)

安装virtualenv
看到别人布署bottle都是配合virtualenv来的，看了一下[virtualenv的介绍](http://www.virtualenv.org/en/latest/ "virtualenv")，确实比较有用，它可以为你的应用创建一个独立的Python环境，这样如果你布署了几个Python应用，就不需要担心它们各自需要的依赖文件版本不同，甚至它们需要的Python版本也不同。

virtualenv官方建使用 pip-1.3以上版本安装，所以先安装一个pip（当然你也可以直接从源码安装）：

     wget http://peak.telecommunity.com/dist/ez_setup.py> 
     python ez_setup.py> 
     easy_install pip> 
     pip install virtualenv

virtualenv会把所有依赖包安装在一个独立的环境下，所以你需要先建立一个给它专用的目录：

     mkdir venv

针对你目前要布置的Python应用，建立一个新的环境目录：

     virtualenv venv/bottle

激活：
     [www@HJY-SERVER ~]$ source ./venv/bottle/bin/activate> 
     (bottle)[www@HJY-SERVER ~]$

激活后你可以看到上面的Shell提示符前面有了你当前环境名称“bottle”。

安装bottle：

     (bottle)[www@HJY-SERVER ~]$ pip install bottle> 
     Downloading/unpacking bottle> 
       Downloading bottle-0.11.6.tar.gz (60kB): 60kB downloaded> 
       Running setup.py egg_info for package bottle> 
     Installing collected packages: bottle> 
       Running setup.py install for bottle> 
         changing mode of build/scripts-2.6/bottle.py from 664 to 775> 
         changing mode of /home/www/venv/bottle/bin/bottle.py to 775> 
     Successfully installed bottle> 
     Cleaning up...

安装uWSGI:

     wget http://projects.unbit.it/downloads/uwsgi-1.4.9.tar.gz> 
     
     tar zxvf uwsgi-1.4.9.tar.gz> 
     cd ../uwsgi-1.4.9.tar.gz> 
     make

如果遇到一大片的错误位置在pythong_plugin.c ,那么应该是缺少python-devel包，用yum安装一下：

     plugins/python/python_plugin.c:1871: error: statement with no effect> 
     yum install python-devel

编译完成后配置：

     mkdir opt/uwsgi> 
     cp uwsgi /opt/uwsgi/> 
     touch /opt/uwsgi/bottle.xml> 
     
     mkdir /home/www/html/cgi/

写一个测试页面：

    cat >> /home/www/html/cgi/index.py
    
    from bottle import route, run, default_app
    
    @route('/')
    def index():
     return "hello world"
    
    if __name__ == "__main__":
     run(host="localhost", port=8888)
    else:
     application = default_app()


uwsgi启动参数配置脚本：

     [root@HJY-SERVER ~]# cat /opt/uwsgi/bottle.ini> 
     [uwsgi]> 
     socket=/opt/uwsgi/run/bottle.sock> 
     daemonize=/opt/uwsgi/run/bottle.log> 
     pidfile=/opt/uwsgi/run/bottle.pid> 
     gid=www> 
     uid=www> 
     virtualenv=/home/www/venv/bottle> 
     chdir=/home/www/html/cgi> 
     module=index> 
     master=True> 
     processes=4> 
     max-requests=5000

uWSGI没有自带的启动脚本，我这里从nginx的启动脚本改了一个过来：

    
    
     
    #!/bin/sh
    #
    # uwsgi        Startup script for uwsgi
    #
    # chkconfig: - 85 15
    # processname: uwsgi
    # config: /etc/uwsgi/uwsgi.conf
    # config: /etc/sysconfig/uwsgi
    # pidfile: /var/run/uwsgi.pid
    #
    ### BEGIN INIT INFO
    # Provides: uwsgi
    # Required-Start: $local_fs $remote_fs $network
    # Required-Stop: $local_fs $remote_fs $network
    # Default-Start: 2 3 4 5
    # Default-Stop: 0 1 6
    # Short-Description: start and stop uwsgi
    ### END INIT INFO
    
    # Source function library.
    . /etc/rc.d/init.d/functions
    
    UWSGI=/opt/uwsgi/uwsgi
    CONFFILE=/opt/uwsgi/bottle.ini
    #CONFFILE=/opt/uwsgi/bottle.xml
    
    
    prog=uwsgi
    uwsgi=${UWSGI}
    conffile=${CONFFILE}
    lockfile=/opt/uwsgi/run/bottle.lck
    pidfile=/opt/uwsgi/run/bottle.pid
    logfile=/opt/uwsgi/run/bottle.log
    SLEEPMSEC=100000
    RETVAL=0
    
    start() {
        echo -n $"Starting $prog: "
    
        ${uwsgi} --ini ${conffile}
        #daemon --pidfile=${pidfile} ${uwsgi} -x ${conffile} -d ${logfile}
        RETVAL=$?
        echo
        [ $RETVAL = 0 ] && touch ${lockfile}
        return $RETVAL
    }
    
    stop() {
        echo -n $"Stopping $prog: "
        killproc -p ${pidfile} ${prog}
        RETVAL=$?
        echo
        [ $RETVAL = 0 ] && rm -f ${lockfile} ${pidfile}
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
        ${uwsgi} -t -c ${conffile} $FLAG
        RETVAL=$?
        return $RETVAL
    }
    
    rh_status() {
        status -p ${pidfile} ${uwsgi}
    }
    
    # See how we were called.
    case "$1" in
        start)
            rh_status >/dev/null 2>&1 && exit 0
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
            if rh_status >/dev/null 2>&1; then
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


把uWSGI加入系统自启动脚本中：

     [root@HJY-SERVER ~]# chkconfig --add uwsgi> 
     [root@HJY-SERVER ~]# chkconfig --level 345 uwsgi on> 
     [root@HJY-SERVER ~]# service uwsgi status

修改nginx.conf文件，支持不同端口的虚拟主机，一个80端口用来支持PHP,一个8051端口支持uWSGI：

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
        listen       8051;
        server_name  localhost;
                root   /home/www/html/cgi;
        location / {
                        include uwsgi_params;
                        uwsgi_pass unix:///opt/uwsgi/run/bottle.sock;
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
}
 

参考文章：

http://www.liuts.com/post/216/
http://jinghong.iteye.com/blog/1283984
http://www.actkr.com/?p=791#sthash.gMK8MScq.3f8JF7RD.dpbs
## Comments

**[kongove](#25361 "2013-06-14 20:10:12"):** 最近博客很活跃呀，还改版了。

