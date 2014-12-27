title: Python实现线程池
tags:
  - Python
id: 313
categories:
  - 编程相关
date: 2008-09-28 11:16:41
---

前言：
        关于线程池(thread pool)的概念请参考http://en.wikipedia.org/wiki/Thread_pool_pattern。在Python中使用线程是有硬伤的，因为Python(这里指C语言实现的Python)的基本调用都最后生成对应C语言的函数调用，因此Python中使用线程的开销太大，不过可以使用Stackless Python(Python的一个修改版)来增强Python中使用线程的表现。
        同时由于Python中GIL的存在，导制在使用多CPU时Python无法充分利用多个CPU，目前pysco这个模块可以针对多CPU提高Python的效率。

在C语言里要实现个线程池，就要面对一堆的指针，还有pthread这个库中那些看起来很让人头痛的一些函数：
int pthread_create(pthread_t *restrict thread,
              const pthread_attr_t *restrict attr,
              void *(*start_routine)(void*), void *restrict arg);
而如果用Python来实现一个线程池的话就好多了，不仅结构十分清晰，而且代码看起来会很优美：

	import  threading 
	from  time  import  sleep 

	class  ThreadPool: 

	     """Flexible  thread  pool  class.   Creates  a  pool  of  threads,  then 
	     accepts  tasks  that  will  be  dispatched  to  the  next  available 
	     thread.""" 

	     def  __init__(self,  numThreads): 

	         """Initialize  the  thread  pool  with  numThreads  workers.""" 

	         self.__threads  =  [] 
	         self.__resizeLock  =  threading.Condition(threading.Lock()) 
	         self.__taskLock  =  threading.Condition(threading.Lock()) 
	         self.__tasks  =  [] 
	         self.__isJoining  =  False 
	         self.setThreadCount(numThreads) 

	     def  setThreadCount(self,  newNumThreads): 

	         """  External  method  to  set  the  current  pool  size.   Acquires 
	         the  resizing  lock,  then  calls  the  internal  version  to  do  real 
	         work.""" 

	         #  Can't  change  the  thread  count  if  we're  shutting  down  the  pool! 
	         if  self.__isJoining: 
	             return  False 

	         self.__resizeLock.acquire() 
	         try: 
	             self.__setThreadCountNolock(newNumThreads) 
	         finally: 
	             self.__resizeLock.release() 
	         return  True 

	     def  __setThreadCountNolock(self,  newNumThreads): 

	         """Set  the  current  pool  size,  spawning  or  terminating  threads 
	         if  necessary.   Internal  use  only;  assumes  the  resizing  lock  is 
	         held.""" 

	         #  If  we  need  to  grow  the  pool,  do  so 
	         while  newNumThreads  >  len(self.__threads): 
	             newThread  =  ThreadPoolThread(self) 
	             self.__threads.append(newThread) 
	             newThread.start() 
	         #  If  we  need  to  shrink  the  pool,  do  so 
	         while  newNumThreads  < len(self.__threads): 
	             self.__threads[0].goAway() 
	             del  self.__threads[0] 

	     def  getThreadCount(self): 

	         """Return  the  number  of  threads  in  the  pool.""" 

	         self.__resizeLock.acquire() 
	         try: 
	             return  len(self.__threads) 
	         finally: 
	             self.__resizeLock.release() 

	     def  queueTask(self,  task,  args=None,  taskCallback=None): 

	         """Insert  a  task  into  the  queue.   task  must  be  callable; 
	         args  and  taskCallback  can  be  None.""" 

	         if  self.__isJoining  ==  True: 
	             return  False 
	         if  not  callable(task): 
	             return  False 

	         self.__taskLock.acquire() 
	         try: 
	             self.__tasks.append((task,  args,  taskCallback)) 
	             return  True 
	         finally: 
	             self.__taskLock.release() 

	     def  getNextTask(self): 

	         """  Retrieve  the  next  task  from  the  task  queue.   For  use 
	         only  by  ThreadPoolThread  objects  contained  in  the  pool.""" 

	         self.__taskLock.acquire() 
	         try: 
	             if  self.__tasks  ==  []: 
	                 return  (None,  None,  None) 
	             else: 
	                 return  self.__tasks.pop(0) 
	         finally: 
	             self.__taskLock.release() 

	     def  joinAll(self,  waitForTasks  =  True,  waitForThreads  =  True): 

	         """  Clear  the  task  queue  and  terminate  all  pooled  threads, 
	         optionally  allowing  the  tasks  and  threads  to  finish.""" 

	         #  Mark  the  pool  as  joining  to  prevent  any  more  task  queueing 
	         self.__isJoining  =  True 

	         #  Wait  for  tasks  to  finish 
	         if  waitForTasks: 
	             while  self.__tasks  !=  []: 
	                 sleep(.1) 

	         #  Tell  all  the  threads  to  quit 
	         self.__resizeLock.acquire() 
	         try: 
	             self.__setThreadCountNolock(0) 
	             self.__isJoining  =  True 

	             #  Wait  until  all  threads  have  exited 
	             if  waitForThreads: 
	                 for  t  in  self.__threads: 
	                     t.join() 
	                     del  t 

	             #  Reset  the  pool  for  potential  reuse 
	             self.__isJoining  =  False 
	         finally: 
	             self.__resizeLock.release() 

	class  ThreadPoolThread(threading.Thread): 
	      """  Pooled  thread  class.  """ 

	     threadSleepTime  =  0.1 

	     def  __init__(self,  pool): 

	         """  Initialize  the  thread  and  remember  the  pool.  """ 

	         threading.Thread.__init__(self) 
	         self.__pool  =  pool 
	         self.__isDying  =  False 

	     def  run(self): 

	         """  Until  told  to  quit,  retrieve  the  next  task  and  execute 
	         it,  calling  the  callback  if  any.   """ 

	         while  self.__isDying  ==  False: 
	             cmd,  args,  callback  =  self.__pool.getNextTask() 
	             #  If  there's  nothing  to  do,  just  sleep  a  bit 
	             if  cmd  is  None: 
	                 sleep(ThreadPoolThread.threadSleepTime) 
	             elif  callback  is  None: 
	                 cmd(args) 
	             else: 
	                 callback(cmd(args)) 

	     def  goAway(self): 

	         """  Exit  the  run  loop  next  time  through.""" 

	         self.__isDying  =  True 

这段100多行的代码完成了一个可动态改变的线程池，并且包含了详细的注释，[这里是代码的出处。我觉得这段代码比Python官方给出的<a href="http://pypi.python.org/pypi/threadpool/1.2.2/">那个](http://code.activestate.com/recipes/203871/)还要好些。他们实现的原理都是一样的，使用了一个队列(Queue)来存储任务。

关于Python中线程同步的问题，[这里](http://effbot.org/zone/thread-synchronization.htm)有不错的介绍。
## Comments

**[crazyfranc](#4396 "2008-10-03 14:41:39"):** 你们公司主要用python做开发？

**[luguo](#4379 "2008-09-28 17:45:26"):** 也不怎么简单嘛 ～～ thread sucks... :-)

**[Amankwah](#4380 "2008-09-28 18:34:57"):** C对Thread的支持有问题吗？这个应该看内核和pthread库以及其他库怎么样吧？ 我最近一直线程着呢，感觉Linux的线程确实不怎么样，我现在的感觉是它在凑合～

**[可可熊](#4438 "2008-10-10 11:30:08"):** 俺们公司用C++； 老大是越来越老糊涂了，俺什么时候说C对Thread的支持有问题！！

**[可可熊](#4439 "2008-10-10 11:30:39"):** 老大说Linux的线程不怎么样，你的意思是Windows的好还是Mac的好！！

