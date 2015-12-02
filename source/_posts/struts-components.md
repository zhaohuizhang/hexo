title: Struts components
tags:
  - component
  - struts
id: 70
categories:
  - Struts
date: 2015-04-23 08:31:23
---

<table>
<thead>
<tr>
<th>名称</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>Action</td>
<td>控制器的一部分，用于模型的交互，执行状态改变或状态查询，以及告诉ActionServlet下一个选择的视图。</td>
</tr>
<tr>
<td>ActionForm</td>
<td>状态改变的数据</td>
</tr>
<tr>
<td>ActionForward</td>
<td>用户指向或视图选择</td>
</tr>
<tr>
<td>ActionMapping</td>
<td>状态改变事件</td>
</tr>
<tr>
<td>ActionServlet</td>
<td>控制器，接收用户请求或状态改变，以及发出视图选择</td>
</tr>
</tbody>
</table>

###  1，ActionServlet

ActionServlet的功能是将一个用户端的URI映射到一个相应的Action类，如果这个类是第一次被调用，那么实例化Action类放到内存中。
<pre>initInternal();
initOther();
intiServlet();
getServletContext().setAttribute();
initModuleConfigFactory();
ModuleConfig moduleConfig = initModuleConfig("",cofing);
initModuleMessageResources(moduleConfig);
initModuleDataSources(moduleConfig);
initModulePlugIns(moduleConfig);
moduleCofing.freeze();</pre>

###  2，ActionForm

ActionForm 提供想要的缓冲、校验、转换机制。本质上是一种JavaBean，是专门用来传递数据的DTO（Data Transfer Object）它用于表单数据验证的validate()方法和用于数据复位的reset()方法。

![](http://images.cnblogs.com/cnblogs_com/eflylab/200701/20060108033310.jpg)

ActionForm有请求（request）和会话（session）两种作用域（scope）.

在验证ActionForm时，如果检测到一个或多个验证错误，Struts会返回在struts-config.xml中&lt;action&gt;定义的input属性指向的页面。

DynaActionForm的目的就是减少ActionForm的数据，利用它用户不用创建一个个具体的ActionForm类，而在配置文件中配置出所需要的虚拟ActionForm。
<pre>&lt;form-beans&gt;
&lt;form-bean name="loginForm" type="org.apache.struts.action.DynaActionForm"&gt;
&lt;form-property name="actionClass" type="java.lang.String" /&gt;
&lt;form-property name="username" type="java.lang.String" /&gt;
&lt;form-property name="password" type="java.lang.String" /&gt;
&lt;/form-bean&gt; 
&lt;/form-beans&gt;</pre>
<pre>String userName = (String) form.get('username');</pre>
验证的部分后面讲

###  3，ActionForward

基类有四个属性：name, path, redirect, classname. ActionForward控制程序的走向。代表一个程序的URI。

当redirect=false时，将保存存储在http请求和请求的上下文的所有内容，仅在同一个应用中可用。redirect=true是，Web客户端进行一次新的http请求，请求的资源可以在同一个应用中，也可以不在，原来的请求参数不再保存，原来的请求上下文也被消除，新http请求仅包含ActionForward的path属性里所包含的参数。

### 4，ActionMapping

&lt;action-mappings&gt;用来描述一组ActionMapping对象，当中的每一个&lt;action&gt;标签都对应一个ActionMapping对象，当客户端发出请求至RequestProcessor时，请求的URI对应于&lt;action&gt;中的path属性，而要呼吁的Action对象则是type属性所设定的对象，RequestProcessor会将请求的执行工作交给盖Action对象来执行。

### 5，Action

每个Action为客户完成某种操作。一旦正确的Action实例确定，就会调用RequestProcessor类的processActionPerform()方法。负责调用Action实例的execute()方法。

ForwardAction 类似于&lt;jsp:forward&gt;,将请求转发到其他的MVC组件，当仅仅是完成从一个页面到另一个页面的请求转发操作是，并不一定需要一个真正的Action，
<pre>&lt;action path="/index" name="loginActionForm" scope="request" tpye="org.apache.struts.action.ForwardAction" parameter="/login.jsp" validate="false" imput="/login.jsp"&gt;&lt;/action&gt;</pre>
DispatchAction, 在一个Action中完成一组紧密相关的业务操作，从而减少重复性编程，使应用更加便于维护。parameter=method , method 对应于Action的具体方法。

MappingDispatchAction类似于DispatchAction

LookupDispatchAction, 必须重写getKeyMethodMap方法，该方法返回一个Map对象。

IncludeAction，引入页面与ForwardAction类似

SwitchAction，