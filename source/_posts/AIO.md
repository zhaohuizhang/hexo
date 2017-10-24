title: AIO
date: 2015-12-03 09:50:20
tags: NIO
---

## AIO Programing
NIO 2.0 引入了新的异步通道的概念，并提拱了异步文件通道和异步套接字通道的实现。异步通道通过两种方式获取操作结果。
* 通过java.util.concurrent.Future类来表示异步操作的结果。
* 在执行异步操作的时候传入一个java.nio.channel。
CompletionHandler 接口的实现类作为操作完成的回调。
NIO 2.0 的异步套接字通道是真正的异步非阻塞I/O，它对应UNIX网络编程中的时间驱动I/O（AIO），他不需要通过多路复用器Selector对应注册的通道进行轮询操作即可以实现异步读写。

[Github](https://github.com/zhaohuizhang/java-nio-demo)
