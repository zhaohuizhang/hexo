title: JAVA-NIO-Netty
date: 2015-12-09 16:32:04
tags: NIO
---

## Java NIO 问题
* NIO 的类库和API繁琐，使用麻烦，需要熟练掌握Selector、ServerSocketChannel、SocketChannel、ByteBuffer等。

* 需要具备其他的额外技能做铺垫，例如熟悉Java多线程编程。因为NIO编程涉及到Reactor模式。

* 可靠性能力补齐、工作量大、难度大。例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常码流的处理能力等问题。

* JDK NIO的BUG。例如epoll bug。它会导致Selector空轮询，最终导致CPU 100%。

## Netty

* API 简单，开发门槛低；

* 功能强大，预设置了多种编码能力，支持多种主流协议；

* 定制能力强，可以通过ChannelHandler对通信框架进行灵活地扩展；

* 性能高，通过与其他业界主流的NIO框架对比，Netty的综合能力最优；

* 成熟、稳定、Netty修复了已经发现的所有JDK NIO BUG，业务人员不需要再为NIO 的BUG烦恼；

* 社区活跃，版本迭代周期短，发现的BUG可以被及时修复，同时，更新的新功能会加入；

* 经历了大规模的商业应用考验，质量得到保证。
