title: Struts Login Demo
tags:
  - Login
  - struts
id: 61
categories:
  - Struts
date: 2015-04-22 12:30:41
---

应用场景：当用户访问某一个应用程序之前，必须先进入登陆界面，进行登陆验证。

![](http://images.51cto.com/files/uploadimg/20090916/1330450.jpg)

Struts的核心功能是前端控制器，前端控制器是一个Servlet，在web.xml中配置所有Request都必须经过前端控制器。前端控制器的名字是ActionServlet，由框架来实现和管理。所有的视图和业务逻辑隔离都是因为这个ActionServlet，可以把它理解为分发器，Dispatcher。

Struts中以XML配置代替硬编码配置信息。以前决定JSP往哪个Servlet提交，是要卸载JSP代码中，也就是说一点这个提交路径要改，就必须改写代码再重新编译。而Struts中编码的只是一个逻辑名字，它对应的class文件写进了xml配置文件中，这个配置文件记录着所有的映射关系，一旦需要改变路径只需要改变XML文件。这也就是Struts-config.xml文件的重用性。

前端控制器从Struts-config.xml中查找与请求匹配的内容，然后进行相关操作。

path值为/login，对应login.do的操作。当login.do的请求到来之后，用一个类型为LoginAction的对象来处理这个请求，在处理请求时需要用一个名为loginActionForm的对象来封装请求的内容。处理后的结构再提交给前端控制器，由前端控制器根据forward的值将相应的内容发送至客户端。

Action为后端控制器，DispatchAction，DynaValidationAction.等等。都有一个execute()方法，用来处理业务逻辑，request , response, formBean, actionMapping4个对象，放回actionForward对象。

在Struts中ActionServlet通过读取struts-config.xml中的配置信息来决定，用户的请求发送给哪个Action对象。每个Action的映射都是通过一个&lt;Action&gt;元素来配置。

这些配置在系统启动时被读入内存，供Struts在运行期间使用。在内存中每个&lt;action&gt;元素对应一个struts.action.ActionMapping类的实例。

1,Web应用启动初始化ActionServlet

2,ActionServlet从struts-config.xml中读取配置信息，把它们存放到各个配置对象中。

![](http://img.ddvip.com/2013/0813/201308130950471776.gif)

&nbsp;