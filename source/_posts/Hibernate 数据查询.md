title: Hibernate 数据查询
tags:
  - HQL
  - QBC
  - Query
  - stored procedure
id: 155
categories:
  - Hibernate
date: 2015-05-07 09:19:25
---

HQL（Hibernate Query Language, HQL），它是Hibernate的面向对象的查询语言。

1，创建查询对象
```
Query query = session.createQuery("from Dept");
```

2，执行查询列出结果
(1)列出所有的结果

调用Query对象的list()方法可以返回所有结果的列表。
```
List<Dept> depts = query.list();

for(Dept dept:depts){}

```

(2)列出单个结果
```
Query query = session.createQuery("from Dept where id=1");

query.setMaxResults(1);

Dept dept = (Dept) query.uniqueResult();

```

(3)迭代访问结果
```
Query query  = session.createQuery("from Dept");

Iterator<Dept> it = query.iterate();

while(it.next()){}

```

3，HQL基本语法

(1)选择要查询的持久化类
```
String hql = "from Dept as d"
```

Dept为持久化类名。区分大小写.

(2)投影查询

与SQL语句类似，HQL可以只查询出某一个或某几个属性，用select关键字来指定查询的属性名。Select id,name from Dept;

可以构造两个属性参数的构造函数。
```
public DeptRow(Long id, String name){}

Select new DeptRow(id,name) from Dept;
```

(3)where 条件句

*   比较运算：=、<、>、 =、等等
*   范围运算：in、 not in、 between、 not between
*   字符串模式匹配：like，"%" 代表任意长度的字符串，代表任意单个字符。
*   逻辑运算符：and、or、not
*   is empty、is not empty
*   upper(s), lower(s),concat(s1,s2),substring(s,offset,length),trim(s),length(s),locate(search,s,offset),abs(n),sqrt(n),mod(dividend,divisor)求余数,size(c)返回集合元素的个数,current_date(),current_time(),current_timestamp(),year(d)month(d)day(d)hour(d)minute(d)second(d)

(4)绑定查询参数

动态绑定查询参数或者字符串拼接（又注入的安全问题）

按照参数名字绑定，“:”
```
String hql = "from Dept where id> :id and name like :likename";
List<Dept> depts = session.createQuery(hql).setLong("id", new Long(inputId)).setString("likename", "%"+inputName+"%").list();

```
按参数位置绑定，"?"
```
String hql = "from Dept where id> ? and name like ?";
List<Dept> depts = session.createQuery(hql).setLong(0, new Long(inputId)).setString(1, "%"+inputName+"%").list();
```
(5)distinct过滤重复值

(6)聚集函数

*   count()
*   avg()
*   sum()
*   max()
*   min()
(7) order by 对结果排序

默认是升序，关键字asc表示升序，desc表示降序。

(8)group by对记录分组

(9)having 对分组后数据进行条件过滤
```
String hql = "select e.dept.name from Employee e group by e.dept having count(e)>1"

```
4，分页查询

当批量查询数据时，如果数据量非常大，怎么办？

*   setFistResult(int firstResult):设置从第几条数据开始查询
*   setMaxResults(int maxResult)：设置每次查询的返回的最大对象数
```
public static List<Employee> findEmployees(int pageNo, int pageSize){
    SessionFactory sessionFactory = new Configuration().configure().buildSessionFactory();
    Session session = sessionFactory.openSession();
    session.beginTransaction();
    String hql = "from Employee"
    List<Employee> empls = session.CreateQuery(hql).setFirstResult((pageNo-1)*pageSize).setMaxResults(pageSize).list();
    session.getTransaction().commit();
    session.close();
    return empls;
}
```
5，批量修改和删除

Hibernate3中新增update 和delete语句。
```
Query query = session.createQuery("delete Dept d where d.name like :likename").setString("likename","%三%");
int count = query.executeUpdate();
```
6，连接查询

类似于SQL，支持连接查询
```
inner join(内连接)：可简写为join。列出这些相关联的实体类中满足连接条件的对象。
left outer join(左连接)：可简写为left join。不仅列出来这些相关联实体类中满足连接条件的对象，而且还包含连接左边的实体类中所有符合搜索条件(where 条件或者having 条件)
right outer join(右连接)：right join。不仅列出来这些相关联实体类中满足连接条件的对象，而且还包含连接右边的实体类中所有符合搜索条件(where 条件或者having 条件)
full join(全连接)
```
7，抓取连接查询

fetch来做数据抓取，所谓抓取，是指从数据库加载一个对象的数据时，同时把它所关联的对象和集合的数据都一起加载出来，以便减少SQL语句的数据，从而提高查询效率。
```
String hql = "FROM Dept d LEFT JOIN FETCH d.employees e WHERE d.name LIKE '%web%'";
List<Dept> list = session.createQuery(hql).list();
Set<Dept> depts = new HashSet<Dept> (list);
for(Dept dept : depts){
for(Employee empl : dept.getEmployees()){
empl.getLoginName();
}
}

```
8，子查询
```
String hql = "FROM Dept d WHERE (SELECT COUNT(E) FROM d.employees e)>1";
List<Dept> list = session.createQuery(hql).list();
for(Dept dept : list){
dept.getName;
}
```
9，命名查询

命名查询是指将HQL查询语句编写在映射文件里，在程序中通过Session的getNameQuery()方法来获取。
```

<hibernate-mapping>
<class name="org.napu.Dept"></class>
<query name="findDeptsByCondition">
<![CDATA[from Dept d where d.name like :likename]]>
</query>
</hibernate-mapping>
List<Dept> depts = session.getNamedQuery("findEdeptsByCondition").setString("likeName","%a%").list();
```

##  Criteria Queries

Criteria叫标准化条件查询，它是一种比HQL更加面向对象的查询语言。QBC（Query By Criteria），特别适当在运行时才能创建查询条件的查询。程序开发时不知道，程序运行时才知道。

### Native SQL Queries

1，实体查询
```

String sql = "SELECT * FROM dept WHERE id > :id limit 3";
List<Dept> depts = session.createSQLQuery(sql).addEntity(Dept.class).setLong(“id”,1).list();
for(Dept dept : depts){
dept.getName();
}

//_____
String sql = "Select {d.*}, {e.*} FROM dept d JOIN employee e ON d.id = e.dept_id";
List list = session.CreateSQLQuery(sql).addEntity("d",Dept.class).addEntity("e",Employee.class).list();
for( Iterator it = list.iterator(); it.hasNext();){
    Object[] obj = (Object[]) iterator.next();
    Dept dept = (Dept)obj[0];
    Employee empl = (Employee) obj[1];
}

```
2，标量查询

将查询结果转化为标量值。.addScalar("countNum",Hibernate.LONG)

3，定义成命名查询来使用
```

<sql-query name="findDeptAndEmployee">
<![CDATA[Select {d.*}, {e.*} FROM dept d JOIN employee e ON d.id = e.dept_id]]>
<return alias="d" class="org.napu.Dept"/>
<return alias="e" class="org.napu.Employee"/>
</sql-query>
```

### 调用存储过程

1，编写过程
```
create procedure select_depts_by_likename(in likeName varchar(20))
begin
    select * from dept where name like likeName;
end;
```
2，把过程映射为命名查询
```
<sql-query name="findDeptAndEmployee">
<return alias="d" class="org.napu.Dept">
<return-property name="id" column="id"/>
<return-property name="name" column="name"/>
<return-property name="createdTime" column="created_time"/>
</return>
{call select_depts_by_likename(:likeName)}
</sql-query>
```
3，调用过程
```
List<Dept> depts = session.getNamedQuery("findEdeptsByCondition").setString("likeName","%a%").list();
```
