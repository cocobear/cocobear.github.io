title: "随手笔记[apache]"
tags:
  - Apache
  - Linux
id: 48
categories:
  - Linux
date: 2007-04-06 19:18:31
---

Monday, 19\. March 2007, 05:41:01
关于apache:

error_log文件中出现下列错误：

[Mon Mar 19 13:31:31 2007] [error] [client 192.168.11.12] (13)Permission denied: access to /home/image/index.jpg denied

     Permission denied
     
     A Permission denied error in the error_log, accompanied by a Forbidden message to the client usually indicates a problem with your filesystem permissions, rather than a problem in the Apache HTTP Server configuration files. Check to make sure that the User and Group running the child processes has adequate permission to access the files in question. Also check that the directory and all parent directories are at least searchable for that user and group (i.e., chmod +x).

[FROM](httpd.apache.org/docs/2.2/faq/error.html)
