title: 操纵Session
tags:
  - Hibernate
  - Session
id: 144
categories:
  - Hibernate
date: 2015-05-05 05:28:31
---

Session接口可以说是Hibernate框架中使用最多的一个接口，它负责持久化工作、负责管理持久化对象的声明周期、提供第一级别的高级缓存来保证持久化对象的数据和数据库同步。

1，Session的缓存

Java中，缓存通常是指Java对象的属性占用的内存空间，一般使用集合类型的属性来作为缓存。Session这一级别的缓存通常称之为一级缓存，是由它的实现类SeeionImpl中的成员属性persistenceContext中定义的一系列Java集合（Map）属性构成。

当程序调用Session的CRUD方法以及调用查询几口list(),iterate()和filter()方法时，如果Session缓存中不存在对象，则加入第一级缓存，如果Session中已经存在这个对象，则不需要去数据库架子而是直接调用缓存，减少数据库访问的频率，提高程序效率。

当调用Transaction的commit()事务提交方法时，会自动进行缓存清理和数据库同步。

Session的缓存一般交由Hibernate框架自动管理而无需程序员干预。

2，持久化生命周期

一个持久化类的实例，在持久化生命周期中会在不同状态之间转变。Hibernate定义四种状态。

![](http://img.dnbcw.info/2011129/3715650.gif)

1，瞬时状态(transient)

该实例是用new创建的，还没有被持久化，不处于任何Session的缓存中，它没有对象标识符。不跟任何一个Session关联，在数据库中没有对应的记录。

2，持久化状态(persistent)

已经被持久化，加入到Session缓存中。实例目前与某个Session关联。它拥有对象标识符值，并且可能在数据库中找到一个对应的行。Hibernate保证在同一个Session实例的缓存中，数据库中的每条记录只对应唯一一个持久化实例。持久化对象总是被一个Session实例关联。持久化实例和数据库中的相关记录对应，Session在清理缓存时，会根据持久化实例的属性数据变化，同步更新数据库。

3，移除状态(removed)

如果一个对象已经被计划在一个Session中结束时删除，它就处于移除状态，但仍然处于Session的缓存中，直到工作单元结束。

4，托管(detached)

已经被持久化过，但已经不处于Session的缓存中。不再位于Session的缓存中，但它拥有对象标识符。

Session的基本操作

public Serializable save(Object obj) throws HibernateException：持久化瞬时实例，返回对象标识符。

public Object get(Class clazz, Serializable id) throws HibernateException：根据制定OID找到一个持久化类。

public Object loadClass clazz, Serializable id) throws HibernateException：类似于get

public void delete(Object object) throws HibernateException：把指定的持久化类实例变成顺时状态，并从数据库表中移除对应的记录。

public void update(Object object) throws HibernateException：重附托管对象，并把它的状态更新到数据库表中。

public void saveOrUpdate(Object obj) throws HibernateException： 同时具有save()和update()的功能

public Object merge(Object object) throws HibernateException：将给定实例的状态复制到具有相同标识符的持久化实例上，并返回这个持久化实例。