title: 24小时才找到的一个bug
id: 105
categories:
  - 编程相关
date: 2007-05-18 16:23:32
tags:
---

由于写FTP服务器时有多处需要用到socket套接字，也为了以后写Linux下的网络程序方便就把socket套接字进行了封装。

安装socket套接的的工作原理把socket一套函数(socket,bind,listen,accept,connect)封装为三个函数：_listen,_accept,_connect。

_listen函数中首先使用socket创建一个套接字，因为是在本机监听，所以使用的地址就是本机的IP地址。然后使用bind把socket生成的socket_fd与本机的一个端口号绑定，这个端口号为_listen的唯一参数。最后使用listen开始监听，本函数返回一个文件描述符，也就是socket产生的文件描述符。

_accept函数中直接调用系统的accept函数阻塞自己，直到有客户请求就接受请求，并且返回一个connect_fd的文件描述符（由accept产生），该函数的参数为_listen返回的文件描述符。

_connect函数与_listen函数类似也是先使用socket创建一个套接字，不过这里使用的地址就是服务器的地址与端口号，而不是本机地址与端口，这两个由该函数的参数给出。接下来它就使用connect去连接指定服务器的端口。

在测试的时候发现了一个bug，_accept返回的值一直为0，昨天中午开始找了半天的时间也没有发现问题，最后无耐之下使用了全局变量，才使用整个程序能够正常运行，不过这样一来就没有达到我封装socket的目的，而且也没有找到bug的原因，今天中午继续查bug，结果发现即使我封装的_accept只有一行语句:
return accept(socket_fd,(struct sockaddr *)&client_addr,&len);
仍然出错，最后看了一下调用_accept那一块的代码，原来问题出在下面的代码：

     35 		if ((connect_fd = _accept(socket_fd,client) == -1)) {> 
        36 			exit(0);> 
        37 		}

竟然找了N长时间没有发现！
## Comments

**[cocobear](#185 "2007-05-20 15:53:34"):** **TO:Amankwah** 那就再仔细看看

**[windflush](#186 "2007-05-18 17:50:16"):** 罪过阿，浪费了时间阿。

**[Amankwah](#187 "2007-05-20 21:55:37"):** 原来不就是个优先级的问题嘛，再次汗～

**[Amankwah](#188 "2007-05-20 10:18:39"):** 我还是没看出来是什么问题，汗～

