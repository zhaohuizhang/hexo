title: XML Schema 和 DTD的区别
tags:
  - DTD
  - Schema
  - XML
id: 204
categories:
  - Java
date: 2015-05-15 12:12:23
---

# [Schema和DTD的区别](http://www.cnblogs.com/zhaozhan/archive/2010/01/04/1639194.html)

<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body">

      Schema是对XML文档结构的定义和描述，其主要的作用是用来约束XML文件，并验证XML文件有效性。DTD的作用是定义XML的合法构建模块，它使用一系列的合法元素来定义文档结构。它们之间的区别有下面几点：

1、Schema本身也是XML文档，DTD定义跟XML没有什么关系，Schema在理解和实际应用有很多的好处。

2、DTD文档的结构是“平铺型”的，如果定义复杂的XML文档，很难把握各元素之间的嵌套关系；Schema文档结构性强，各元素之间的嵌套关系非常直观。

3、DTD只能指定元素含有文本，不能定义元素文本的具体类型，如字符型、整型、日期型、自定义类型等。Schema在这方面比DTD强大。

4、Schema支持元素节点顺序的描述，DTD没有提供无序情况的描述，要定义无序必需穷举排列的所有情况。Schema可以利用xs:all来表示无序的情况。

5、对命名空间的支持。DTD无法利用XML的命名空间，Schema很好满足命名空间。并且，Schema还提供了include和import两种引用命名空间的方法。

</div>
<div class="clear"></div>
<div id="blog_post_info_block">
<div id="BlogPostCategory">分类: [XML](http://www.cnblogs.com/zhaozhan/category/209171.html)</div>
</div>
</div>