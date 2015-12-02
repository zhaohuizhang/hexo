title: Struts Tiles
tags:
  - struts
  - Tiles
id: 106
categories:
  - Struts
date: 2015-04-30 02:44:25
---

Struts Tiles 框架提供了一种模板机制，模板定义了网页的布局，同一模板可以被多个Web页面公用。Tiles框架还允许定义可重用的Tiles组件，它可以描述一个完整的网页，也可以描述网页的局部内容。
<pre>iframe框架中也可以嵌入多个页面，但对于每个请求一个页面，服务器就要开启一个线程，也就是说，采用iframe框架，当用户第一次向服务器请求页面时，服务器要为这一次请求开启多个线程。而Tiles框架，将多个页面组合成一个页面，每次请求，服务器只要开启一个线程它既实现了iframe的布局功能，也解决了服务器负载问题
</pre>
Tiles框架增强了基于组件的设计和Web UI设计中的模板概念。接触WebUI组件之间的耦合并重用它们。Tiles模板的优点：

1，创建可重用的模板

2，动态构建和装在页面

3，定义可重用的Tiles组件

4，支持国际化

Tiles建立在JSP的include指令的基础上，但比JSP的include指令更加强大。包括以下内容：

*   Tiles标签库
*   Tiles组件的配置文件
*   TilesPlugIn插件
Tiles的insert标签取代JSP include指令来创建复合式页面，还不能充分发挥Tiles框架的优势。将Tiles的组件代码移植到tiles-defs.xml文件中。

集成Struts与Tiles

1.  创建工程，设置Tiles环境
2.  设置页面
3.  设置XML文件在WEB-INF中添加tiles-defs.xml文件
4.  修改index.jsp文件
5.  在Struts-config.xml中配置TilesPlugin插件
6.  发布运行
如果Tiles组件代表完整的网页，可以通过Struts Action来调用Tiles组件。
<pre>&lt;action-mappings&gt;
    &lt;action path="/index" type="org.apache.struts.actions.ForwardAction" parameter="index-definition"&gt;
    &lt;/action&gt;
&lt;/action-mappings&gt;</pre>
ForwardAction把请求转发给名为index-definition的Tiles组件，能够充分利用Struts框架的流程控制功能，又能减少JSP文件的数目。