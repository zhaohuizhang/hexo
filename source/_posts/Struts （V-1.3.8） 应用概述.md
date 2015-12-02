title: Struts （V-1.3.8） 应用概述
tags:
  - MVC
  - struts
id: 51
categories:
  - Struts
date: 2015-04-21 14:19:59
---

struts 为 Apache Jakarta项目的组成部分。项目的创立者希望通过对该项目的研究，改进和提高Java Server Pages(JSP), Servlet、标签库以及面向对象的技术水准。

Struts名字来源于建筑和旧式飞机中使用的支持金属架。它的目的是为了减少运用MVC设计模式开发Web应用的时间。混合使用Servlets和JSP的优点来建立可扩展的应用。

Framework, 软件架构的建立是一个复杂而又持续改进的过程，开发者要尽量重用以前的框架。

MVC是Model-View-Controller是一种常用的设计模式。MVC减弱了业务逻辑接口和数据接口之间的耦合

![](http://huoding.com/wp-content/uploads/2011/05/web_mvc.gif)

Struts是MVC的一种实现，他将Servlet和JSP标记用作实现的一部分。

![](http://s2.sinaimg.cn/bmiddle/535e2244476b8d011e3b1&amp;690)

Controller：有一个XML文件Struts-config.xml，与之相关联的是Controller，在Struts中，承担MVC中Controller角色的是一个Servlet，叫ActionServlet。ActionServlet是一个通用的控制组件，这个控制组件提供了处理所有发送到Struts的HTTP请求的入口点。它截取和分发这些请求到相应的动作类（这些动作类都是Action类的子类）。另外控制组件也负责用相应的请求参数填充ActionForm（FormBean），并传给动作类（ActionBean）。动作类用于实现核心商业逻辑，可以访问JavaBean或调用EJB。最后动作类把控制权传给后续的JSP文件，后者生成视图。所有这些控制逻辑利用Struts-config.xml文件来配置。

View：主要由JSP生成页面完成视图，Struts提供了丰富的JSP标签库：HTML，Bean，Logic，Template等，这有利于充分表现逻辑和程序逻辑。

Model：模型以一个或多个JavaBean的形式存在。这些Bean分为三类：Action Form，Action，JavaBean or EJB。Action Form通常称为FormBean，封装了来自于Client的请求信息，如表单信息。Action通常称为ActionBean，获取从ActionServlet传来的FormBean，取出并处理FormBean中的相关信息，一般是调用Java Bean 或 EJB等。

在Struts中，用户的请求一般以*.do作为请求服务名，所有的*.do请求均被指向ActionSevlet, ActionServlet根据Struts-config.xml中配置信息，将用户请求封装成一个指定名称的FormBean，并将此FormBean传至指定名称的ActionBean，由ActionBean完成相应的业务操作，如文件操作、数据库操作等。每一个*.do均有对应的FormBean名称和ActionBean名称，这些在Struts-config.xml中配置。

Struts和核心是ActionServlet，ActionSevlet的核心是struts-config.xml.

当请求来临之时，系统如何知晓要用哪个ActionForm来封装请求数据，又是如何知晓要用哪个Action来处理数据，最后又是如何来应答的呢？找struts-config.xml

Struts用配置文件来定义应用的一些内容，包括连接的逻辑名称。Struts在启动时读入配置文件，以创建一个所需要的对象数据库。各种Struts组件都引用这个数据库来提供框架的各种服务。