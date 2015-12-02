title: ORM（Object Relation Mapping）
tags:
  - Hibernate
  - ORM
id: 123
categories:
  - Java
date: 2015-05-03 09:38:47
---

## 前言

对象关系映射，是一种完成对象模型到关系模型的映射技术。就是一种把应用程序的对象数据持久化到关系数据库表的一种技术。

*   把对象的数据转储到关系型数据库表中时就会发生如下不匹配的问题。
*   对象模型中对象之间的关联关系与关系模型中数据库表之间的关系无法一一对应。
*   对象模型中对象的继承关系在关系模型中无法直接表示。
*   对象模型中对象的等值性在关系模型数据库表中表示困难。
*   对象模型中有关联的对象之间的导航访问在关系模型中无法直接实现。
ORM 来解决上述问题。它能用面向对象的思想开发基于关系型数据库的应用程序。

### Hibernate

JBoss公司著名架构师Gavin King设计，开发的一个开源的ORM框架。

*   它是连接Java应用程序和关系数据库的中间件。
*   它对JDBC API进行了封装，负责Java对象的持久化。
*   在分层的软件框架中它位于持久化层，封装的所有的数据访问细节，使业务逻辑层可以专注于实现业务逻辑。
*   它是一种ORM工具，能够建立面相对象的域模型和关系模型的映射。

###  hibernate 的核心类和接口

#### 1，Configuration类

Configuration类是Hibernate的入口，它负责配置并启动Hibernate。Hibernate框架通过Configuration实例加载配置文件信息，然后读取指定对象关系文件的内容并创建SessionFactory实例。

#### 2，SessionFactory接口

SessionFactory接口负责初始化Hibernate，一个SessionFactory实例对应一个数据存储源（一般就是指一个数据库）。应用程序从SessionFactory中获取Session实例。SessionFactory具有以下几个特点：

*   线程安全，即同一个SessionFactory实例可以被应用的多个线程共享。
*   它是重量级的，因为它需要一个很大的缓存，用来存放预定义的SQL语句以及映射元数据。
所以说，如果一个应用程序只访问一个数据库，则只需要创建一个全局的SessionFactory实例。

#### 3，Session接口

Session是Hibernate中应用最频繁的接口。Session也被称为持久化管理器。他负责管理所有与持久化相关的操作：存储、更新、删除和加载对象等。Session具有以下特点。

单线程，非共享的对象。线程不安全，在设计软件架构时，应该避免多个线程共享同一个Session实例。

Session实例是轻量级的，它的创建和销毁不需要消耗太多的资源。可以为每个请求分配一个Session实例，在每次请求过程中及时创建和销毁Session实例。

Session有一个缓存，他存放当前工作单元加载的对象。Session的缓存被称为Hibernate的第一级缓存。

#### 4，Transaction接口

Transaction接口是Hibernate框架的事务接口。它对底层事务接口做了封装，包括：JDBC API和JTA。这样，使得Hibernate应用可通过一致的Transaction接口来声明事务边界，这有助于应用程序在不同环境和容器中移植。

#### 5，Query和Criteria接口

他们是Hibernate的查询接口，用于从数据库存储源查询对象以及控制执行查询的过程。Query包装了一个HQL的查询语句；而Criteria接口完全封装了基于字符串形式的查询语句，比Query更加面相对象，Criteria接口擅长执行动态查询。

### Hibernate的工作过程

![](http://img.my.csdn.net/uploads/201304/07/1365329836_2944.jpg)