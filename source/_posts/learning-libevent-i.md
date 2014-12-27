title: libevent学习-(I)
tags:
  - C
  - libevent
id: 512
categories:
  - 编程相关
date: 2009-02-23 13:57:32
---

     [libevent](http://monkey.org/~provos/libevent/)是一个异步事件处理软件函式库，以BSD许可证释出。> 
     
     libevent提供了一组应用程序编程接口（API），让程式设计师可以设定某些事件发生时所执行的函式，也就是说，libevent可以用来取代网络服务器所使用的循环检查架构。

摘自[维基百科](http://zh.wikipedia.org/wiki/Libevent)

[http://blog.gslin.info/2005/11/network-programming-using-libevent-i.html](http://blog.gslin.info/2005/11/network-programming-using-libevent-i.html)
这里介绍了libevent相关的网络编程背景，需要带套访问哦。

以下分析针对libevent-1.4.3-stable。

来看libevent自带的例子：


	/*
	 * Compile with:
	 * cc -I/usr/local/include -o event-test event-test.c -L/usr/local/lib -levent
	 */

	static void
	fifo_read(int fd, short event, void *arg)
	{
		char buf[255];
		int len;
		struct event *ev = arg;

		/* Reschedule this event */
		event_add(ev, NULL);

		fprintf(stderr, &quot;fifo_read called with fd: %d, event: %d, arg: %pn&quot;,
			fd, event, arg);
		len = read(fd, buf, sizeof(buf) - 1);

		if (len == -1) {
			perror(&quot;read&quot;);
			return;
		} else if (len == 0) {
			fprintf(stderr, &quot;Connection closedn&quot;);
			return;
		}

		buf[len] = '�';
		fprintf(stdout, &quot;Read: %sn&quot;, buf);
	}

	int
	main (int argc, char **argv)
	{
		struct event evfifo;
		struct stat st;
		const char *fifo = &quot;event.fifo&quot;;
		int socket;

		if (lstat (fifo, &amp;st) == 0) {
			if ((st.st_mode &amp; S_IFMT) == S_IFREG) {
				errno = EEXIST;
				perror(&quot;lstat&quot;);
				exit (1);
			}
		}

		unlink (fifo);
		if (mkfifo (fifo, 0600) == -1) {
			perror(&quot;mkfifo&quot;);&lt;code&gt;&lt;/code&gt;
			exit (1);
		}

		/* Linux pipes are broken, we need O_RDWR instead of O_RDONLY */
		socket = open (fifo, O_RDWR | O_NONBLOCK, 0);

		if (socket == -1) {
			perror(&quot;open&quot;);
			exit (1);
		}

		fprintf(stderr, &quot;Write data to %sn&quot;, fifo);
		/* Initalize the event library */
		event_init();

		/* Initalize one event */
		event_set(&amp;evfifo, socket, EV_READ, fifo_read, &amp;evfifo);

		/* Add it to the active events, without a timeout */
		event_add(&amp;evfifo, NULL);

		event_dispatch();
		return (0);
	}
	[/c]

	我把原来代码中WIN平台相关的去掉了，看起来方便一些，这个例子创建了一个pipe，并且使用libevent来监听是否可读，读有数据可读时调用函数fifo_read。
	libevent调用比较简单，首先event_init()对event库进行初始化，然后使用event_set()来对某个fd的操作进行监听，接着使用event_add()把这个event激活，这里可以指定超时的时间，最后一步event_dispatch()，开始进行循环。

	event_init()里只是对外的一个接口，这个函数调用了event_base_new()，分配了一个event_base类型的空间，设置了一些全局变量，使用detect_monotonic来检测是否支持CLOCK_MONOTONIC类型的时钟，这里不太理解为什么要使用clock_gettime(CLOCK_MONOTONIC, &ts)来获得当前时间，这个与gettimeofday得到的精度是一样的，只是一个是标准的时间（UNIX元年算起），一个是开机时间算起，有什么差别吗？

	[ CLOCK_MONOTONICI测试代码：](#)

	/* gcc -o ftime  ftime.c*/ 
	#include&lt;stdio .h&gt;
	#include&lt;time .h&gt;
	#include&lt;sys /time.h&gt;

	int main(void)
	{
	    struct timeval tp;

	    gettimeofday(&amp;tp,NULL);
	    printf(&quot;sec=%ld\n&quot;,tp.tv_sec);

	    struct timespec ts;

	    clock_gettime(CLOCK_MONOTONIC,&amp;ts);
	    printf(&quot;sec=%ld\n&quot;,ts.tv_sec);
	    return 0;
	}



结果：

     [cocobear@cocobear libevent-1.4.3-stable]$ gcc ftime.c -lrt> 
     [cocobear@cocobear libevent-1.4.3-stable]$ ./a.out > 
     sec=1235372220> 
     sec=4621

接下来就检测可使用的事件检测函数，这里与系统相关的调用被封装成了一个结构eventop：

	struct eventop {
	    const char *name;
	    void *(*init)(struct event_base *);
	    int (*add)(void *, struct event *);
	    int (*del)(void *, struct event *);
	    int (*dispatch)(struct event_base *, void *, struct timeval *);
	    void (*dealloc)(struct event_base *, void *);
	    /* set if we need to reinitialize the event base */
	    int need_reinit;
	};

编译时libevent会通过

	#ifdef HAVE_SELECT
	    &amp;selectops,
	#endif

来“动态”的确定eventops数组，在定义这个eventops数组时确定了使用这些事件驱动模型的顺序，如果你机子上有多种可用的模式，则可以通过修改改数组来自定义使用的模型。

在event_base_new()的最后调用了event_base_priority_init()初始化了消息的优先级队列。主要就是对activequeues变量进行空间分配。默认是分配了一个event_list给activequeues。
## Comments

**[山客](#8575 "2010-09-19 16:16:52"):** monotonic和标准时间在实际使用的差别是，标准时间会correct，从而可能造成定时器问题

**[duşakabin](#13349 "2012-03-16 11:32:38"):** Thanks for this fantastic post, I am glad I found this site on yahoo.

