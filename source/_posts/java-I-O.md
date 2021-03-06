title: java-I/O-1
date: 2015-11-27 18:26:10
tags: javaIO
---

# Java 的 I/O 演进之路(1)

Java 是 Sun Microsystems 公司在1995年首先发布的编程语言和计算平台。
Java 之所以能够得到如此广泛的应用，除了摆脱硬件平台的依赖具有“一次编写、到处运行”的平台无关性之外，另一个重要原因是：丰富而强大的类库以及众多第三方开源类库，使得开发Java程序更加简单和便捷。
    
## 1.1 I/O 基础入门
Java 1.4 之前的早期版本，Java 对 I/O 的支持并不完整，开发人员在开发高性能I/O程序的时候，会面临一些巨大的挑战和困难，主要问题如下：
   
* 没有数据缓冲区，I/O性能存在问题；
* 没有C或者C++中的Channel概念，只有输入和输出流；
* 同步阻塞式I/O通信（BIO），通常会导致线程被长时间阻塞；
* 支持的字符集有限，硬件可移植性不好。

### 1.1.1 Linux 网络I/O模型
Linux的内核将所有外部设备都当成一个文件来操作，对一个文件的操作会调用内核提供的系统命令，返回一个file（fd，文件描述符）。而对一个socket的读写，也会有相应的描述符，成为socketfd，描述符就是一个数字，他指向内核的一个结构体（文件属性，数据区等一些属性）。
   
根据UNIX网络编程对I/O模型的分类，UNIX提供5种I/O模型，分别如下：
* 阻塞I/O模型：缺省条件下都是阻塞I/O模型，所有的文件操作都是阻塞的。以套接字接口为例：在进程空间中调用recvfrom，其系统调用制动数据包到达且被复制到应用进程的缓冲区中或者发生错误时才返回，在此期间一直会等待，进程从调用recvfrom开始到它返回的整段时间内都是被阻塞的，因此被称为阻塞I/O。
* 非阻塞I/O模型：recvfrom从应用层到内核的时候，如果该缓冲区没有数据的话，就直接返回一个EWOULDBLOCK错误，一般都对非阻塞I/O模型进行轮询检查这个状态，看内核是否有数据过来。
* I/O复用模型：Linux提供select/poll，进程通过将一个或多个fd传递给select或者poll系统调用，阻塞在select操作上，这样select/poll可以帮我们侦测多个fd是否处于就绪状态。select/poll是顺序扫描fd是否就绪，而且支持的fd数量有限，因此它的使用受到了一些制约。Linux还提供了一个epoll系统调用，epoll使用基于事件驱动方式代替顺序扫描，当有fd就绪时，立即回调函数rollback。
* 信号驱动I/O模型：首先开启套接口信号驱动I/O功能，并通过系统调用sigaction执行一个信号处理函数（此系统调用立即返回，进程继续工作，它是非阻塞的）。当数据准备就绪是，就为该进程生成一个SIGIO信号，通过信号回调通知应用程序调用recvfrom来读取数据，并通知主循环函数处理数据。
* 异步I/O：告知内核启动某个操作，不让内核在整个操作完成后通知我们。与信号驱动模型区别：喜好驱动I/O由内核通知我们何时可以开始一个I/O操作；异步I/O模型由内核通知我们I/O操作何时已经完成。

### 1.1.2 I/O多路复用技术
在I/O编程过程中，当需要同时处理多个客户端接入请求时，可以利用多路线程或者I/O多路复用技术来处理。I/O多路复用把多个I/O阻塞复用到同一个select的阻塞上，从而使得系统在单线程的情况下同时处理多个客户端的请求。与传统的多线程/多线程模型比，I/O复用的最大优势是系统开销小，系统不需要创建新的额外的进程或者线程，也不需要维护这些线程或进程的运行，降低了系统的维护工作量，节省了系统资源，I/O多路复用的主要应用场景如下：
1. 服务器需要同时处理多个处于监听状态或者多个连接状态的套接字；
2. 服务器需要同时处理都中网络协议的套接字；

目前支持I/O多路复用的系统调用有select, pselect, poll, epoll,在Linux网络编程中，很长一段时间都使用select做轮询和网络事件通知，Linux的新版本用epoll。起改进如下：
* 一个进程打开的socket描述符（FD）不受限制，仅受限于操作系统的最大文件句柄数。
select最大的缺陷就是单个进程所打开的FD是有一定限制的，它由FD_SETSIZE设置，默认是1024.对于那些需要支持上万个TCP链接的大型服务器来说显然是太少了。可以选择修改这个宏然后重新编译内核，不过这会带来网络效率的下降。我们也可以通过选择多进程的方案（传统的Apache方案）解决这个问题，不过虽然在Linux上创建进程的代价比较小，但人就是不可忽视的，另外，进程的数据交换非常麻烦，对于Java由于没有共享内存，需要通过Socket通信或者其他方式进行数据同步，这带来了额外的系能损耗，增加了程序复杂度，所以也不是一种完美的解决方案。epoll并没有这个限制，它所支持的FD上限是操作系统的最大文件句柄数，这个数字远大于1024.cat /proc/sys/fs/file-max，这个值跟系统的内存有关。
* I/O的效率不会随着FD数目的增加而直线下降
传统的select/poll另一个知名的弱点就是当你拥有一个很大的socket集合，由于网络延迟时或者链路空闲，任意时刻只有少部分的socket是“活跃”的，但是select/poll每次都是扫描全部集合，导致效率的线性下降。epoll不存在这个问题，它只会对“活跃”的socket进行操作-这是因为在内核实现中epoll是根据每个fd上面的callback函数实现的，那么，只有“活跃”的socket才会主动去调用callback函数，其他idle状态socket则不会。epoll实现了一个伪AIO。
* 使用mmap加速内核与用户空间的消息传递
无论是select，poll还是epoll都需要内核把FD消息通知给用户空间，如何避免不必要的内存负值就显得非常重要，epoll是通过内核和用户mmap同一块内存实现。
* epoll 的API更加简单
