title: Hibernate 基本配置及操作
tags:
  - Configuration
  - Hibernate
id: 135
categories:
  - Hibernate
date: 2015-05-04 09:14:14
---

### Hibernate 全局配置文件

1，hibernate.properties；hibernate.cfg.xml;

hibernate框架在启动的时候会在应用的ClassPath路径中查询有没有这两个文件，如果有则加载他们的配置参数。

在hibernate.cfg.xml文件中，首先配置session-factory元素来指定一个SessionFactory的配置，
<pre>&lt;hibernate-configuration&gt;
&lt;session-factory&gt;
&lt;property name="hibernate.dialect"&gt;org.hibernate.dialect.MySQLDialect&lt;/property&gt;
&lt;/session-factory&gt;
&lt;/hibernate-configuration&gt;</pre>
2， JDBC属性

*   hibernate.connection.url
*   hibernate.connection.driver_class
*   hibernate.connection.username
*   hibernate.connection.password
*   hibernate.connection.isolation：设置JDBC事务的隔离级别
*   hibernate.connection.batch_size：批量操作的记录数
3，连接池属性

通常要使用第三方的连接池技术。C3P0这个开源的连接池产品为例进行链接数据库参数的配置。
<pre>&lt;property name="hibernate.c3p0.min_size"&gt;5&lt;/property&gt;
&lt;property name="hibernate.c3p0.max_size"&gt;20&lt;/property&gt;
&lt;property name="hibernate.c3p0.timeout"&gt;1800&lt;/property&gt;
&lt;property name="hibernate.c3p0.max_statement"&gt;100&lt;/property&gt;</pre>
4，缓存属性

用来配置Hibernate使用的二级缓存策略。常用的配置如下：

*   hibernate.cache.provider_class：指定第三方的缓存提供类的名称。ehcache缓存插件可做为Hibernate的二级缓存提供者。org.hibernate.cache.EhCacheProvider
*   hibernate.cache.use_query_cache：指定是否开启Hibernate的查询缓存。可选值为false和true。默认为false。
5，其他属性

*   hibernate.show_sql：指定是否把Hibernate运行时的SQL语句输出到控制台。
*   hibernate.format_sql：指定是否对Hibernate运行时产生的SQL语句进行格式化便于阅读。
*   hibernate.hbm2ddl.auto：指定应用程序在运行时，当产生SessionFactory实例时对是否自动检查数据库结构，或者将数据库schema的DDL导出到数据库。可选值：Validate（检查数据库结构），update（数据库结构发生变化时修改），create（将数据库schema的DDL导出到数据库），create_drop（在SessionFactory中实例创建时间数据schema的DDL导出到数据库，在SessionFactory被显示关闭时将数据库自动删除）。
*   hibernate.current_session_context_class：为当前Session指定一个策略。常用值：jta(当前Session根据JTA来跟踪和界定)、Thread（前Session通过当前执行的线程来跟踪和界定）

###  对象关系映射文件

Hibernate的对象关系映射文件把面相对象中的实体类对象映射到数据库中的实体，把实体类之间的关联关系也映射到数据库中多个表之间的相互关系中。
<pre>XML文件的起始行，指定XML的版本和编码方式
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
映射文件的所使用的DTD声明
&lt;!DOCTYPE hibernate-mapping PUBLIC
 "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
 "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd"&gt;

&lt;!-- Hibernate对象关系映射文件的根元素 --&gt;
&lt;hibernate-mapping&gt;
 &lt;!-- class元素用来定义一个持久化类及对应的数据库表 --&gt;
 &lt;class name="com.qiujy.domain.Account" table="account"&gt;
 &lt;!-- 
 id元素：指定每个持久化类的唯一标识(即对象标识符OID)到数据库表主键字段的映射 
 name属性：指定持久化类的OID名
 column属性：指定数据库表主键字段名。此属性的名和映射到数据库表的字段名相同时，可省略
 type属性：指定主键映射时所使用的Hibernate类型名。此属性的类型为基本数据类型和String类型时，可省略
 --&gt;
 &lt;id name="id" column="id" type="long"&gt;
 &lt;!-- generator元素：指定对象标识符的生成器名。
 native生成器把对象标识符值的生成工作交由底层数据库来完成
 --&gt;
 &lt;generator class="native" /&gt;
 &lt;/id&gt;
 &lt;!-- 
 property元素：指定持久化类的每一个要映射到数据库表的字段的属性的信息。
 name属性；指定持久化类中要映射到数据库表字段的属性名。
 column属性：指定对应的表的字段名。此属性的名和映射到数据库表的字段名相同时，可省略
 type属性：指定属性映射所使用的Hibernate类型名。此属性的类型为基本数据类型和String类型时，可省略
 not-null属性：指定此属性映射到数据库表的字段值是否允许为值 
 --&gt;
 &lt;property name="loginname" column="loginname" type="string" not-null="true"/&gt;
 &lt;property name="password" column="password" type="string" not-null="true"/&gt;
 &lt;property name="email" column="email" type="string"/&gt;
 &lt;!-- 属性类型为Date类型的必须要明确指定使用的Hibernate类型名 --&gt;
 &lt;property name="registrationTime" column="registration_time" type="timestamp"/&gt;
 &lt;/class&gt;
&lt;/hibernate-mapping&gt;</pre>
1,映射持久化类

在Hibernate对象关系映射文件中，使用class元素来映射持久化类到数据库表中

*   name：指定要映射的持久化类的名称
*   table：指定和此持久化类对应的数据库表名
2，映射对象标识符

Hibernate中要求每一个持久化类都有一个对象标识符来唯一标识它的每一个实例。对象标识符的值是数据库表中行的主键。

3，对象标识符生成方式

可以通过ID元素的generator子元素，指定对象标识符的生成器。

1.  代理对象标识符：在一个持久化类中添加一个没有业务意义的属性来作为对象标识符，这样应用程序的领域模型更具有扩展性。
2.  自然对象标识符：由许多遗留的SQL数据模型使用了带有业务意义的主键组成。

*   如果对象标识符的数据类型为整数型（Long，int，short）或对应的包装类型，为提高应用程序在不同数据库上的移植能力，建议使用native。
*   如果对象标识符的数据类型的为字符串型，为提高应用程序的在不同数据库上的移植能力，建议用UUID。
*   如果应用程序是先有数据库的物理模型后偶建立实体模型，且使用了自然主键，那么选择assigned.
4，映射普通属性

对象标识符，版本号，自定义数据类型属性以外的属性。通过class元素的property子元素来映射&lt;property name="属性名" column="列名称" type=“映射属性”/&gt;

5，Hibernate映射的数据类型

![](http://www.educity.cn/article_images/2014-03-18/11810dd6-1bfb-4478-92ca-471f8942ba92.jpg)

&nbsp;