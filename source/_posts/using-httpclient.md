title: 使用HTTPClient
id: 136
categories:
  - 编程相关
date: 2007-06-13 17:05:05
tags:
---

Windows下的环境变量配置：

**
JAVA_HOME=C:\Program Files\Java\jdk1.6.0_01
PATH=%JAVA_HOME%\bin;%PATH%
CLASSPATH=.\;%JAVA_HOME%\lib\tools.jar;%JAVA_HOME%\lib\commons-httpclient-3.0.1.jar;%JAVA_HOME%\lib\commons-logging-1.1.jar;%JAVA_HOME%\lib\commons-codec-1.3.jar;%JAVA_HOME%\lib\junit-4.3.1.jar;%JAVA_HOME%\lib\commons-logging-api-1.1.jar;%JAVA_HOME%\lib\commons-logging-api-1.1.jar;%JAVA_HOME%\lib\commons-logging-api-1.1.jar;%JAVA_HOME%\lib\commons-logging-adapters-1.1.jar**

上面要用到的几个包下载地址：
[httpclient](http://jakarta.apache.org/commons/httpclient/downloads.html)
[](http://jakarta.apache.org/commons/codec)commons-codec
[](http://jakarta.apache.org/commons/logging)commons-logging
[](http://www.junit.org)junit

下载binary的包，解压后把jar文件放在lib目录下。

BTW：**下面这篇文章对Java类路径进行了详细的解释，终于把这个搞明白了[了解 Java 类路径](http://www.ibm.com/developerworks/cn/db2/library/techarticles/dm-0408anderson2/)

**