title: NIO-Programing
date: 2015-12-10 10:57:35
tags: NIO
---

## NIO 开发步骤
* 创建ServerSocketChannel，配置他为非阻塞模式；
* 绑定监听，配置TCP参数，backlog的大小；
* 创建一个独立的I/O线程，用于轮询多路复用器Selector；
* 创建Selector，将之前创建的ServerSocketChannel 注册到Selector上，监听SelectorKey.ACCEPT;
* 启动I/O线程，在循环体中执行Selector。select()方法，轮询就绪的Channel；
* 当轮询到就绪状态的Channel时，需要进行判断，如果是OP_ACCEPT状态，说明是新接入的客户端，则调用ServerSocketChannel.accept()方法接受新的客户端；
* 设置新接入的客户端链路SocketChannel为非阻塞模式，配置其他的一些TCP参数；
* 将SocketChannel注册到Selector，监听OP_READ操作位；
* 如果轮询的Channel为OP_READ，则说明SocketChannel中有就绪的数据包需要读取，则构造ByteBuffer对象，读入数据包；
* 如果轮询的Channel为OP_WRITE，说明还有数据没有发送完成，需要继续发送。
