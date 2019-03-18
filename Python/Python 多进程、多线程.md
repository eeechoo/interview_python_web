# Python 多进程、多线程
线程间资源是共享的，讲安全：信号量，锁，原子操作

进程间资源是独立的，讲通讯：管道，共享内存

## 进程间通信方式
下图取自 The Linux Programming Interface---chapter 43
![Inter-Process Communication, IPC](./__images/IPC.PNG)


[mutex 与 semaphore 的区别](https://blog.csdn.net/panzhenjie/article/details/10083035)




## Python IPC

[Exchanging objects between processes](https://docs.python.org/3/library/multiprocessing.html#exchanging-objects-between-processes)

## Python threading
https://docs.python.org/3/library/threading.html#module-threading

- lock
- RLcok
[举例讲解 Python 中的死锁、可重入锁和互斥锁](http://python.jobbole.com/82723/)
- Condition Object
也是锁，只是多了 notify 和 wait 功能
[python多线程--Condition(条件对象)](https://www.cnblogs.com/thunderLL/archive/2018/10/23/9838984.html)
- semaphore


- Event
- Barrier

- threading.Thread
- Timer

## Python multi-process
