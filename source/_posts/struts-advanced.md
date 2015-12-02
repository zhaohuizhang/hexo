title: Struts Advanced
tags:
  - struts
  - Tokens
  - 中文乱码
id: 112
categories:
  - Struts
date: 2015-04-30 07:02:02
---

### 1，利用Token解决重复提交

基本原理：服务器在处理到达的请求之前，会将请求中包含的令牌值与保存在当前用户会话中的令牌值进行比较，看是否匹配。在处理完请求后，且在响应发送给客户端之前，将会产生一个新的令牌值，该令牌值除了传给客户端以外，也会将会话中保存的旧的令牌值进行替换。这样如果用户后退到刚才的提交页面并重复提交的话，客户端传过来的令牌就和服务器端的令牌不一致，从而有效的防止了重复提交的发生。

### 2，BeanUtils与PropertyUtils

Apache Jakarta Commons项目提供了很多流行的commons组件。

BeanUtils提供了JAVA反射和自省API的包装。其主要目的是利用反射机制对JavaBean的属性进行处理。一个JavaBean通常包含了大量的属性，在很多情况下，对JavaBean的处理会导致大量getset代码堆积。

如果用户有两个具有很多相同属性的JavaBean，一个很常见的现象就是Struts里的PO对象和对应的ActionForm

BeanUtils.copyProperties(admin,adminForm)，支持数据类型范围内的转换。

BeanUtils不对这些属性进行处理，需要手动处理。admin.setAttrabuite。

PropertyUtils工具类，提供copyProperties()方法，不支持数据类型转换，速度快。

时间类型要被转化时，使用java.sql.Date，不要使用java.util.Date(不支持)。

#### <span style="color:#ff0000;">使用BeanUtils的成本很高，超过取数据，将其复制到对应的value 对象，以及通过串行化将其返回到远程客户机的时间的总和。不要滥用。</span>

#### 3，Struts的上传和下载

Commons-fileupload组件，struts上传组件。

Commons-fileupload组件：

配置Web.xml文件，action所对应是一个Servlet，而非Struts的action。编辑上传类，FileUploadServlet.java

Struts上传文件组件：

在form中enctpye的值一定要设置为multipart或form-data

配置Struts-config.xml，formbean,action

ActionForm Bean中必须第一个名为file的属性，org.apache.struts.upload.FormFile类型。

### 4，Null与“”的区别

String str1 = null; Str 引用为空；没有被分配空间，不是对象；if(str1==null)

String str2 = ""; str引用一个空串；分配的空间，是对象；if(str2.equal(""))
<pre>if(str==null||str.equal("")){

    //先判断是不是对象，在判断是不是空字符串

}</pre>

###  5,struts处理中文乱码

*   ##### 页面中显示中文乱码

*   ##### 传递参数中文乱码

*   ##### 国际化资源文件乱码
1,页面中文乱码
<pre>&lt;% @page language="java" import="java.util." pageEncoding="UTF-8"%&gt;</pre>
2,传递中文乱码

a,Tomcat目录中conf/server.xml中
<pre>&lt;Connector port="8080" protocol="HTTP/1.1" connectionTimeout="2000" redirectPort="8443" URIEncoding="UTF-8"&gt;</pre>
b,添加过滤器Filter
<pre>public class CharacterEncodingFilter implements Filter{
    public void destroy(){}
    public void doFilter(ServletRequest request, ServletResponse response FilterChain chain) throws Exceptions{
    request.setCharacterEncoding("utf-8");
    chain.doFilter(request,response);
  }
public void init(FilterConfig arg0) throws ServletException{}
}

web.xml
&lt;filter&gt;
&lt;filter-name&gt;characterEncoding&lt;/filter-name&gt;
&lt;filter-class&gt;org.napu.utils.CharacterEncodingFilter&lt;/filter-class&gt;
&lt;/filter&gt;
&lt;filter-mapping&gt;
&lt;filter-name&gt;characterEncoding&lt;/filter-name&gt;
&lt;url-pattern&gt;/*&lt;/url-pattern&gt;
&lt;/filter-mapping&gt;
过滤的URL为“/*”,表示当前的request请求。</pre>
3，国际化资源文件乱码

native2ascii --encoding gbk ApplicationResources_zh.properties bank.properties