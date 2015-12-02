title: Struts 标签
tags:
  - struts
  - Tag
id: 80
categories:
  - Struts
date: 2015-04-24 08:34:10
---

Strust 提供了5个标签库：HTML标签库，Bean标签库，Logic标签库，Template标签库，Nested标签库。
<table>
<thead>
<tr>
<th>标签库</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>HTML标签</td>
<td>用来创建能够和Struts框架以及其他相应的HTML标签交互的语言环境标签</td>
</tr>
<tr>
<td>Bean标签</td>
<td>在访问JavaBean及其属性，以及定义一个新的bean时使用</td>
</tr>
<tr>
<td>Logic标签</td>
<td>管理条件产生的输出和对象集产生的循环</td>
</tr>
<tr>
<td>Template标签</td>
<td>很少使用,都用Tiles啦</td>
</tr>
<tr>
<td>Nested标签</td>
<td>增强对其他Struts标签的嵌套使用能力</td>
</tr>
</tbody>
</table>
1,Struts HTML标签

&lt;html:html&gt;,&lt;html:base&gt;,&lt;html:img&gt;,&lt;html:link&gt;,&lt;html:errors&gt;,&lt;html:form&gt;,&lt;html:password&gt;,&lt;html:select&gt;,&lt;html:option&gt;,

2，Bean标签

操作JavaBean相关的对象，cookie,header,parameter,define,write,message,include,page,resource,size,struts等。

3，Logic标签

比较运算符标签；循环遍历标签；字符串匹配标签；判断指定内容是否存在标签；判断空标签；转发与重定向标签

&lt;logic:equal&gt;,&lt;logic:greaterThan&gt;,&lt;logic:greaterEqual&gt;,&lt;logic:lessThan&gt;,&lt;logic:lessEqual&gt;,&lt;lgoic:iterate&gt;,&lt;logic:match&gt;,&lt;logic:notmatch&gt;,&lt;logic:present&gt;,&lt;logic:notpresent&gt;

Sample:
<pre>&lt;% pageContext.setAttribute("test","helloworld");%&gt;
&lt;logic:match value="hello" name="test"&gt;
hello is component of test
&lt;/logic:match&gt;</pre>