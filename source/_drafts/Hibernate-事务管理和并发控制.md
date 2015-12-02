title: Hibernate 事务管理和并发控制
tags:
  - 并发访问控制
  - 事务
id: 170
categories:
  - Hibernate
---

1，事务管理

事务就是单个逻辑单元执行的一组数据操作，这些操作要么必须全部成功，要么必须全部失败，以保证数据一致性和完整性。事务具有ACID。

*   原子性（Atomic）:事务由一个或多个行为绑在一起组成，好像是一个单独的工作单元。原子性确保在事务中的所有操作要么完成，要么都不发生。
*   一致性（consistent）:一旦一个事务结束（不管成功与否），系统所处的状态和它的业务规则是一致的。既数据应当不会被破坏。
*   隔离性（Isolated）:事务允许多个用户操作同一条数据，一个用户的操作不会和其他用户的操作相混淆。
*   持久性（Durable）:一旦事务完成，事务的记过应该持久化。
2，数据库事务管理

数据库事务的ACID特性是由关系数据库管理系统（RDBMS）来实现的。数据库管理系统采用日志保证事务的原子性、一致性和持久性，日志记录事务对数据库所做的更新，如果某个事务在执行过程中发生错误，就可以根据日志，撤销事务对数据库已做的更新，使数据库退回到执行事务前的状态。数据库管理系统采用锁的机制来实现事务的隔离性。当多个事务同时更新数据库同一个数据时，只允许持有锁的事务能更新该数据，其他事务必须等待，知道前一个事务释放了锁，其他事务才有机会更新改数据。

数据库系统自动保证数据的ACID属性。

JDBC API中，java.sql.Connection类代表一个数据库连接。

*   setAutoCommit(true)
*   commit()
*   rollback()
<pre>Connection con = null;
PrepareStatement pstmt = null;
try{
con = DriverManager.getConnection(dbURL,username,password);
con.setAutoCommit(false);
pstmt = ...;
pstmt.executeUpdate();
con.commit()
}catch(Exception e){
com.rollback();
}finally{
}</pre>
3，Hibernate应用程序中的事务管理

Hibernate对JDBC进行轻量级的对象封装，Hibernate本身在设计时并不具备事务处理功能，平时所用的Hibernate的事务，只是将底层的JDBCTransaction，或者JTATransaction进行一下封装，在外面套上Transaction和Session的外壳，其实底层都是通过委托底层的JDBC或JTA来实现事务的调度功能。

(1)Hibernate中使用JDBC事务

要在Hibernate中使用JDBC事务，可以在Hibernate.cfg.xml中指定hibernate事务为JDBCTransaction。

&lt;property name="hibernate.transaction.factory_class"&gt;org.hibernate.transaction.JDBCTransactionFactory&lt;/property&gt;

Hibernate使用JDBC transaction处理方法
<pre>Transaction tx = null;
try{
    tx = session.beginTransaction();
    ...
    tx.commit();
}catch(RuntimeException e){
    throw e;
}finally{
    session.close();
}</pre>
(2)Hibernate中使用JTA事务

Java Transaction API 是事务服务的JavaEE解决方案，本质上，它是描述事务接口的JavaEE模型的一部分。

UserTransaction，TransactionManager，Transaction，这些接口共享公共事务操作。UserTransaction能够执行事务划分和基本的事务操作，TransactionManager能够执行上下文管理。

JTA服务需要第三方应用程序提供具体的实现。

### 并发访问控制

1，数据库事务并发引起的问题

在并发环境，一个数据库系统会同时为各种各样的客户端程序提供服务，也就是说，在同一时刻，会有多个客户程序同时访问数据库系统，这多个客户程序中的事务访问数据库中相同的数据时，如果没有采取必要的隔离机制，就会导致各种各样的并发问题的发生。

*   第一类丢失更新：两个事务都更新同一行，而另外一个事务异常回滚，使两处变化都丢失。这种并发问题是由完全没有隔离事务造成的。
*   脏读：一个事务读到另一个事务未提交的更改数据。
*   不可重复读：一个事物两次读取同一行数据，两次数据不同。
*   第二类丢失更新：这是不可重复读中的特例，一个事务覆盖另一个事务已经提交的数据。
*   幻读：一个事务前后执行一个查询两次，在第二个结果集中包括第一个结果集中不可见的行，或者包括已经删除的行。
2，事务隔离级别

让用户根据在事务隔离性和并发性之间做合理的权衡。

*   Read Uncommitted(读未提交)：可以防止第一类丢失更新问题，但没有解决第一类丢失更新和脏读的并发问题。它的事务隔离性最低。1
*   Read Committed(读已提交)：可以防止脏读一下的并发问题，但没有解决第一类丢失更新，脏读和不可重复读的并发问题。2
*   Repeatable Read(可重复读)：可以防止不可重复读第二类丢失更新和幻读的并发问题，但没有解决幻读问题。4
*   Serializable(串行化)：提供最严格的事务隔离性。它把事务隔离成连续的一个接一个地执行，而不是并发执行。8
在hibernate.cfg.xml中配置隔离级别如下：

&lt;property name="hibernate.connection.isolation"&gt;2&lt;/property&gt;

在开始一个事务之前，Hibernate将为从连接池中获得的JDBC连接设备级别。

3，乐观的并发控制

乐观锁：假定当前事务曹总数据资源是，不会有其他事务同时访问改数据资源，因此不做数据库层次上的锁定。为了维护正确的数据，乐观锁使用应用程序上的版本控制来避免可能出现的并发问题的发生。

唯一能够同时保持高并发和高可伸缩性的方法就是实用带版本话的乐观并发控制。版本检查使用版本号，或者时间戳来检测更新冲突。

Hibernate通过版本号