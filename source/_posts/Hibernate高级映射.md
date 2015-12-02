title: Hibernate高级映射
tags:
  - Hibernate
  - 对象映射
id: 148
categories:
  - Hibernate
date: 2015-05-05 14:49:44
---

### 集合映射

&lt;set java.util.Set or java.util.SortSet&gt;&lt;list java.util.List&gt;&lt;bag java.util.Collection&gt;&lt;idbag&gt;&lt;map java.util.Map or java.util.SortMap&gt;&lt;primitive-array&gt;&lt;array&gt;

### 组件映射

&lt;component&gt;

1，组件类作为持久化类的单个属性

2，组件类作为持久化类的集合属性：集合映射中的类

3，组件类作为持久化类的对象标识符属性：数据库采用联合主键

### 管理关系映射

关联关系是在领域模型建模中经常使用到的一种关系，它是对现实世界中事物之间的关系最基本的表示。

管联关系指的是不同持久化类之间的一种结构关系，简单地说，关联关系描述某个对象在一段时间内一直知道另一个对象的存在。

关联关系包括多样性关联和方向性关联。多样性（一对多，多对多），方向性（单项关联，双向关联）

映射关联关系就是把对象模型中类之间的关联关系映射成关系模型中数据库表之间的外键引用关系。在映射设计时，考虑两个问题：一是如何将对象模型中的对象之间的关系保存在关系模型的数据库表中，二是如何从关系模型的数据库表中检索出对象模型中的关联的对象。

1，单向多对一

多个员工属于同一部门

Department.hbm.xml

Employee.hbm.xml

&lt;many-to-one name="dept" column="dept_id"&gt;

2，单向一对一

Citizen.hbm.xml

&lt;many-to-one name="idCard" column="idcard_id" unique="true" cascade="all"&gt;

IdCard.hbm.xml

每个公民只有一张身份证，这就是典型的一对一关联关系。

3，双向一对一

基于唯一外键的一对一双向关联

Citizen.hbm.xml

&lt;many-to-one name="idCard" column="idcard_id" unique="true" cascade="all"/&gt;

IDCard.hbm.xml

&lt;one-to-one name="citizen" property-ref="idCard"/&gt;

基于主键的双向一对一关联

Citizen.hbm.xml

&lt;generator class="foreign"&gt;&lt;param name="property"&gt;idCard&lt;/param&gt;&lt;/generator&gt;

&lt;one-to-one name="idCard" constrained="true" cascade="all"/&gt;

IDCard.hbm.xml

&lt;one-to-one name="citizen"/&gt;

4，单向一对多

一个账号可以有多个订单
<pre>//Order.java
public class Order{
  private Long id;
  private String orderNo;
  private Date createdTime;
  public Order(){}
}
//Account.java
public class Account{
  private Long id;
  private String loginName;
  private Set&lt;Order&gt; orderSet;
  public Account(){}
}
//Account.hbm.xml
&lt;set name="orderSet" cascade="save-update"&gt;
    &lt;key column="account_id"/&gt;
    &lt;one-to-many class="org.napu.Order"&gt;
&lt;set&gt;</pre>
5，双向一对多（多对一）
<pre>//Order.java
public class Order{
  private Long id;
  private String orderNo;
  private Date createdTime;
  private Account account;
  public Order(){}
}
//Account.java
public class Account{
  private Long id;
  private String loginName;
  private Set&lt;Order&gt; orderSet;
  public Account(){}
}
//Order.hbm.xml
&lt;many-to-one name="account" column="account_id" not-null="true"/&gt;
//Account.hbm.xml
&lt;Set name="orderSet" cascade="all" inverse="true"&gt;
    &lt;key column="account_id"/&gt;
    &lt;one-to-many class="org.napu.Order"&gt;
&lt;/Set&gt;</pre>
6，单向多对多

大学生和课程之间的关系
<pre>//Student.java
public class Student{
  private Long id;
  private String name;
  private String grade;
  private Set&lt;Course&gt; courses;
  public Student(){}
}
//Course.java
public class Course{
  private Long id;
  private String name;
  private Double creditHours;
  public Course(){}
}
//Student.hbm.xml
&lt;set name="courses" table="student_course"&gt;
    &lt;key column="student_id"/&gt;
    &lt;many-to-many column="course_id" class="org.napu.Course"&gt;
&lt;set&gt;</pre>
7，双向多对多
<pre>//Student.java
public class Student{
  private Long id;
  private String name;
  private String grade;
  private Set&lt;Course&gt; courses;
  public Student(){}
}
//Course.java
public class Course{
  private Long id;
  private String name;
  private Double creditHours;
  private Set&lt;Student&gt; students;
  public Course(){}
}
//Student.hbm.xml
&lt;set name="courses" table="student_course"&gt;
    &lt;key column="student_id"/&gt;
    &lt;many-to-many column="course_id" class="org.napu.Course"/&gt;
&lt;set&gt;
//Course.hbm.xml
&lt;set name=""students table="students_course" inverse="true"&gt;
    &lt;key column="course_id"/&gt;
    &lt;many-to-many column="student_id" class="org.napu.Student"/&gt;
&lt;/set&gt;</pre>

###  继承关系映射

继承在对象模型中是is a(是一个)的关系，在关系模型中，实体之间只有has a(有一个)的关系。

Hibernate提供了3中继承映射的方法。
<pre>//Singer.java
public class Singer{
  private Long id;
  private String name;
  private String region;
  private String description;
  pubic Singer(){}
}
//SingleSinger.java
public class SingleSinger extends Singer{
  private Character gender;
  public SingleSinger(){}
}
//Bands.java
public class Bands extends Singer{
  private String leader;
  public Bands(){}
}</pre>
1，整个继承层次一张表
<pre>//Singer.hbm.xml
&lt;hibernate-mapping&gt;
 &lt;class name="org.napu.Singer" table="singer"&gt;
  &lt;id name="id" column="id" type="long"&gt;&lt;generator class="native"/&gt;&lt;/id&gt;
  &lt;discriminator column="type" type="string"/&gt;
  &lt;property name="name"/&gt;
  &lt;property name="region"/&gt;
  &lt;property name="description"/&gt;
  &lt;subclass name="org.napu.SingleSinger" discriminator-value="S"&gt;
    &lt;property name="gender"/&gt;
  &lt;/subclass&gt;
  &lt;subclass name="org.napu.Bands" discriminator-value="B"&gt;
    &lt;property name="leader"/&gt;
  &lt;/subclass&gt;
 &lt;/class&gt;
&lt;/hibernate-mapping&gt;</pre>
2，每个子类一张表
<pre>&lt;joined-subclass name="org.napu.SingleSinger" table="single_singer"&gt;
  &lt;key column="singler_id"/&gt;
  &lt;property name="gender"/&gt;
&lt;/joined-subclass&gt;
...</pre>
3，每个具体类一张表
<pre>&lt;union-subclass name="org.napu.SingleSinger" table="single_singer"&gt;
  &lt;property name="gender"/&gt;
&lt;/union-subclass&gt;
...</pre>
一些经验：

*   如果不需要多态查询：使用每个具体类一张表。
*   一定要使用多态查询：子类中的属性相对较少，使用每个层次一张表。
*   子类中属性较多，使用每个子类一张表。
*   简单的问题一般选择每个层次继承一张表，复杂案例一般选择每个子类一张表