title: NIO-Introduce-Abstractly
date: 2015-11-30 15:07:51
tags: NIO
---

# NIO 入门

## 传统的BIO编程

网络编程的基本模型是Client/Server模型，也就是两个进程之间进行相互通信，其中服务端提供位置信息，客户端通过连接操作向服务器端监听的地址发送连接请求，通过三次握手简历连接，如果连接简历成功，双发就可以通过网络套接字（Socket）进行通信。

在基于传统同步阻塞模型开发中，ServerSocket负责绑定IP地址，启动监听端口；Socket负责发起连接操作。连接成功之后，双发通过输入输出流进行同步阻塞式通信。

在传统的BIO通信模型的服务端，通常由一个独立的Acceptor线程负责监听客户端的连接，它接收到客户端连接请求之后为每个客户端创建一个新的线程进行链路处理，处理完成之后，通过输出流返回应答给客户端，线程销毁。最大的缺点是，当客户端访问量增大之后，服务端的线程个数和客户端的并发访问量呈1：1 的正比关系，由于线程是Java虚拟机非常宝贵的系统资源，当线程膨胀之后，系统的性能将急剧下降，系统会出现线程堆栈溢出、创建线程失败等问题。

为了改进一线程一连接模型，后来又演进出了一种通过线程池或者消息队列实现1个或者多个线程处理N个客户端的模型，由于它的底层通信机制依然使用同步阻塞I/O，所以被称为“伪异步”。

## 伪异步I/O编程

后端通过一个线程池来处理多个客户端的请求接入，形成客户端个数M：线程池最大线程数N的比例关系，其中M远大于N，通过线程池可以灵活调配线程资源，设置线程最大值，防止海量并发接入导致线程耗尽。

当有新的客户端接入的时候，将客户端的Socket封装成一个Task投递到后端线程池中进行处理，JDK的线程池维护一个消息队列和N个活跃线程对消息队列中的消息进行处理。由于线程池可以设置消息队列的大小和最大线程数，因此，它的资源占用是可控的，无论多少客户端并发访问，都不会导致资源的耗尽和宕机。

伪异步I/O通信框架采用了线程池实现，因此避免了为每个请求都创建一个独立线程造成的线程资源耗尽的问题。但是它底层通信仍采用同步阻塞模型，因此无法从根本上解决问题。

Java输入流 InputStream
```
public int read(byte b[]) throws Exception{
	return read(b, 0, b.length);
}
```

当对Socket输入流进行读取时，操作一直会阻塞下去，直到发生如下几件事情：
* 有数据可读；
* 可用数据以读取完毕；
* 发生空指针或者I/O异常。

当对方发送消息或者应答消息比较缓慢、或者网络传输较慢时，读取输入流一方的通信线程将长时间阻塞。

Java输出流 OutputStream
```
public void write(byte b[]) throws IOException
```

当调用OutputStream 的write方法时，它将会被阻塞，直到所有要发送的字节全部写入完毕，或者发生异常。

通过分析输入和输出流的API文档，我们发现读和写操作都是同步阻塞的，阻塞时间取决于对方的I/O线程处理速度和网络I/O的传播速度。

## Non block I/O
与Socket和SocketServer类相对应，NIO 也提供的SocketChannel和ServerSocketChannel两种不同的套接字通道实现.这两种新增的通道都支持阻塞和非阻塞两种模式。阻塞模式使用非常简单，但是性能和可靠性都不好。
非阻塞模式恰好相反。
