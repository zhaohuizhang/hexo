title: Struts 工作原理
tags:
  - struts
  - work principal
id: 66
categories:
  - Struts
date: 2015-04-22 13:14:27
---

![](http://images.51cto.com/files/uploadimg/20090916/1333550.jpg)

1，读取配置（初始化ModuleConfig对象）

采用Struts框架的Web应用，在Web应用启动时就会加载ActionServlet，在ActionServlet初始化ModuleConfig的时候，调用initModuleConfigFactory()初始化配置工厂，然后由配置工厂通过initModuleConfig('',cofing)获得ModuleConfig对象。
<pre>initModuleMessageResources(moduleConfig);
initModuleDataSources(moduleConfig);
initModulePlugIns(moduleCofig);</pre>
这些方法的功能就是：容器在加载Struts应用程序时，会先加载web.xml中与Struts相关的一些参数，找到Struts-config.xml文件，通过循环来读取此文件和解析里面的内容，并初始化相关对象。

2，用户请求

用户提交表单或调用URL向Web应用程序服务器提交一个请求，请求数据用HTTP协议上传给Web服务器。

3，填充FormBean

填充FormBean的过程包括实例化，复位，填充数据，校验，保存等操作。根据*.do请求从ActionConfig中找出对应该请求的Action子类，如有对应的Action且这个Action有一个相应的ActionForm，则ActionForm被实例化并用HTTP请求的数据填充其属性，并保存在ServletContext中，这样它们就可以被其他的Action对象或者JSP调用。

4，转发请求

控制器根据ActionConfig将请求发送到具体的Action，与请求一起的FormBean也一并传给这个Action对象。

5，处理业务

Action一般包含一个execute()方法，负责业务逻辑，执行完毕后返回一个ActionForward对象，控制器通过ActionForward对象进行转发工作。

6，返回响应

Action根据不同的结果返回一个响应给总的控制器，该目标响应对象对应一个具体的JSP页面或另一个Action。

7，查找响应

总控制器根据业务功能Action返回的目标响应对象找到相应的资源对象，通常是一个具体的JSP页面。

8，响应用户

目标相应对象将结果展现给用户目标响应对象，即具体的JSP页面，这样客户就得到响应的结果。