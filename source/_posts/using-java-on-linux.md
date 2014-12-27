title: linux下使用java
tags:
  - Java
  - Linux
id: 47
categories:
  - Linux
date: 2007-04-06 19:18:29
---

Friday, 16\. March 2007, 14:02:55


学校开设了java课，得写些java程序，今天配置了一下linux下的java编译以及运行环境:

首先在java.sun.com下载的jdk1.6.0

[jdk-6-linux-i586.bin](http://192.18.108.139/ECom/EComTicketServlet/BEGIND2D5C9D462B1CCD71DC405A5E5630F63/-2147483648/2007288495/1/790634/790370/2007288495/2ts+/westCoastFSEND/jdk-6-oth-JPR/jdk-6-oth-JPR:5/jdk-6-linux-i586.bin)

建议大家还是下.bin格式的,虽然rpm格式也是可以用的。

配置环境变量的时候还是出了点问题，可能是与环境变量的先后顺序有关系，其实挺简单的，就是在.bashrc文件中加几行东西就ok了,如下：

    JAVA_HOME=/usr/java/jdk1.6.0/
    PATH=$JAVA_HOME/bin:$PATH
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tool.jar:$CLASSPATH
    export JAVA_HOME PATH CLASSPATH


JAVA_HOME是java的主安装目录，PATH里添加java的bin目录，CLASSPATH：如果传递给javac编译器的源文件里引用到的类定义在本文件和传递的其它文件中找不到， 则编译器会按CLASSPATH定义的路径来搜索

最后我又在opera里添加了java支持，只需要添加java的路径就可以了/usr/java/jdk1.6.0，记得要重起一下浏览器，我当时就是因为没有重起浏览器怎么都不行![:smile:](/community/graphics/smilies/smile.gif)

推荐一个不错的java在线小游戏，我是用来测试opera中的java是否可以正常使用，机子太慢了，玩起来_卡_

[http://www.javagameplay.com/](http://www.javagameplay.com/)

