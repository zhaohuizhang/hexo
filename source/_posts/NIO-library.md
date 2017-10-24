title: NIO-library
date: 2015-12-02 13:15:50
tags: NIO
---

## NIO 类库简介

### 缓冲区Buffer
Buffer是一个对象，它包含一些要读出或写入的数据。在NIO库中，所有数据都是用缓冲区处理的。任何时候访问数据都是通过缓冲区进行的。

缓冲区实质上是一个数组。通常它是一个字节数组（ByteBuffer），也可以使用其他种类的数组。
* ByteBuffer
* CharBuffer
* ShortBuffer
* IntBuffer
* LongBuffer
* FloatBuffer
* DoubleBuffer
每一个Buffer类都是Buffer接口的子实例。除了ByteBuffer，每一个Buffer类都有完全一样的操作。

### 通道Channel
Channel是一个通道，可以通过它读取和写入数据。通道与流不同之处在于通道是双向的，流只是在一个方向上移动（一个流必须是InputStream或者是OutputStream的子类）。

因为通道是全双工的，所以它可以更好的映射操作系统底层的API。特别是UNIX网络编程模型中，底层操作系统通道都是全双工的。从类图上看Channel可以分为两大类；分别是用于网络读写的SelectableChannel和用于文件操作的FileChannel。

SocketChannel和ServerSocketChannel都是SelectableChannel的子类。

### 多路复用器Selector
多路复用器提供已经就绪的任务的能力。简单的讲Selector不断轮询注册在其上的Channel，如果某个Channel上面有新的TCP接入、读或者写事件，这个Channel就处于就绪状态，会被Selector轮询出来，然后通过SelectionKey可以获取就绪Channel的集合，进行后续I/O操作。

一个多路复用器Selector可以轮询多个Channel，由于JDK采用epoll()代替传统的select实现，所以它并没有最大连接句柄的限制。

## NIO服务器主要创建过程
* 打开ServerSocketChannel，用于监听客户端请求连接，它是所有客户端连接的父管道
```
ServerSocketChannel acceptorSvr = ServerSocketChannel.open();
```
* 绑定监听端口，设置连接为非阻塞模式
```
acceptorSvr.socket().bind(new InetSocketAddress(InetAddress.getByName("IP"),port));
acceptorSvr.configureBlocking(false);
```
* 创建Reactor线程，创建多路复用器并启动线程
```
Seletor selector = Selector.open();
New Tread(new ReactorTask()).start();
```
* 将ServerSocketChannel注册到Reactor线程的多路复用器Selector上，监听ACCEPT事件
```
SelectionKey key = acceptorSvr.register(selector, SelectionKey.OP_ACCEPT,ioHandler);
```
* 多路复用器在线程run方法的无限循环体内轮询准备就绪的Key
```
int num = selector.select();
Set selectedKeys = selector.selectedKeys();
Iterator it = selectedKeys.iterator();
while(it.hasNext()){
  SelectionKey key = (SelectionKey) it.next();
  //TODO deal with I/O event
}
```
* 多路复用器监听到所有新的客户端接入，处理新的接入请求，完成TCP三次握手，建立物理链路
```
SocketChannel channel = svrChannel.accept();
```
* 设置客户端链路为非阻塞模式
```
channel.configureBlocking(false);
channel.socket().setReuseAddress(true);
```
* 将新的客户端连接注册到Reactor线程的多路复用器上，监听读操作，用来读取客户端发送的网络消息
```
SelectionKey key = socketChannel.register(selector, SelectionKey.OP_READ, ioHandler);
```
* 异步读取客户端请求消息到缓冲区
```
int readNumber = channel.read(receivedBuffer);
```
* 对ByteBuffer进行编解码，如果有半包指针reset,继续读取后续的报文，将解码成功的消息封装成Task，投递到业务线程池中，进行业务逻辑的编排
```
Object message = null;
while(buffer.hasRemain()){
  byteBuffer.mark();
  Object message = decode(ByteBuffer);
  if(message == null){
    byteBuffer.reset();
    break;
  }
  messageList.add(message);
}
if(!byteBuffer.hasRemain()){
  byteBuffer.clear();
}else{
  byteBuffer.compact();
}
if(messageList != null & !messageList.isEmpty()){
  for(Object messageE : messageList){
    handlerTask(messageE);
  }
}
```
* 将POJO对象encode成ByteBuffer，调用SocketChannel的异步write接口，将消息异步发送给客户端
```
socketChannel.write(buffer);
```

## NIO 客户端创建
* 打开SocketChannel，绑定客户端本地地址
```
SocketChannel clientChannel = SocketChannel.open();
```
* 设置SocketChannel为非阻塞模式，同时设置客户端连接的TCP参数
```
clientChannel.configureBlocking(false);
socket.setReuseAddress(true);
socket.setReceiveBufferSize(BUFFER_SIZE);
socket.setSendBufferSize(BUFFER_SIZE);
```
* 异步连接服务器
```
boolean connectioned = clientChannel.connect(new InetSocketAddress("IP").port);
```
* 判断连接是否成功，如果连接成功，则直接注册读状态位到多路复用器中，如果当前没有连接成功（异步连接，返回false，说明客户端已经发送sync包，服务端没有返回ack包，物理链路没有建立）
```
if(connectioned){
  clientChannel.register(selector, SelectorKey.OP_READ, ioHandler);
}else{
  clientChannel.register(selector, SelectorKey.OP_CONNECT,ioHandler);
}
```
* 向Reactor线程的多路复用器注册OP_CONNECT状态位，监听服务端的TCP ACK应答
```
clientChannel.register(selector, SelectorKey.OP_CONNECT, ioHandler);
```
* 创建Reactor线程，创建多路复用器并启动线程
```
Selector selector = Selector.open();
New Thread(new ReactorTask()).start();
```
* 多路复用器在线程run方法的无限循环内轮询准备就绪的Key
```
int num = selector.select();
Set selectedKeys = selector.selectedKeys();
Iterator it = selectedKeys.iterator();
while(it.hasNext()){
  SelectionKey key= (SelectionKey) it.next();
  // TODO deal with I/O event
}
```
* 接收connect事件进行处理
```
if(key.isConnectable()){
  //handlerConnect();
}
```
* 判断连接结果，如果连接成功，注册读事件到多路复用器
```
if(channel.finishConnect()){
  registerRead();
}
```
* 注册事件到多路复用器
```
clientChannel.register(selector, SelectionKey.OP_READ,ioHandler);
```
* 异步读取客户端请求消息到缓冲区
```
int readNumber = channel.read(recevicedBuffer);
```
* 对ByteBuffer进行编解码，如果有半包消息接收缓冲区Reset，继续读取后续的报文，将解码成功的消息封装成Task，投递到业务线程池中
```
Object message = null;
while(buffer.hasRemain){
  byteBuffer.mark();
  Object message = decode(byteBuffer);
  if(message == null){
    byteBuffer.reset();
    break;
  }
  messageList.add(message);
}
if(!byteBuffer.hasRemain()){
  byteBuffer.clear();
}else{
  byteBuffer.compact();
}
if(messageList != null && !messageList.isEmpty()){
  for(Object messageE : messageList){
    handlerTask(messageE);
  }
}
```
* 将POJO对象encode成ByteBuffer，调用SocketChannel的异步write接口，将消息异步发送给客户端
```
socketChannel.write(buffer);
```
### NIO 编程的优点
* 客户端连接都是异步的，可以通过多路复用器注册OP_CONNECT等待后续结果，不需要像之前的客户端那样被同步阻塞。
* SocketChannel的读写操作都是异步的，如果没有可读写的数据，它不会同步等待,直接返回，这样I/O通信线程可以处理其他的链路，不需要同步等待这个链路可用。
* 线程模型的优化，由于JDK的Selector在Linux等主流操作系统上通过epoll实现，它没有连接句柄数的限制，这就意味着一个Selector可以处理成千上万的客户端连接，而且性能不会随着客户端的增加而下降。
