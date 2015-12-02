title: java 集合类
tags:
  - Collections
  - java
id: 225
categories:
  - Java
date: 2015-06-10 08:56:44
---

[![QQ图片20150610153630](https://napuzhang.files.wordpress.com/2015/06/qqe59bbee7898720150610153630.png?w=300)](https://napuzhang.files.wordpress.com/2015/06/qqe59bbee7898720150610153630.png)

&nbsp;

虚线标识“interface”，点线框标识“abstract”类，实线框代表实际的类。

利用iterator()方法，所有集合都能生成一个“反复器”（Iterator）。反复器其实就象一个“枚举”
（Enumeration），是后者的一个替代物，只是：
(1) 它采用了一个历史上默认、而且早在OOP 中得到广泛采纳的名字（反复器）。
(2) 采用了比Enumeration 更短的名字：hasNext()代替了hasMoreElement()，而next()代替了
nextElement()。
(3) 添加了一个名为remove()的新方法，可删除由Iterator 生成的上一个元素。所以每次调用next()的时候，只需调用remove()一次。

### Collections

<pre>Method：

Boolean add(Object)

Boolean addAll(Collection)

void clear()

Boolean contians(Object)

Boolean containsAll(Collection)

Boolean isEmpty()

Iterator iterator()

Boolean remove(Object)

Boolean removeAll(Collection)

Boolean retianAll(Collection)

int size()

Object[] toArray()

Object[] toArray(Object[] a)
</pre>

### Lists

ArrayList：用于替代原先的Vector，允许我们快速的访问元素，但在从列表中删除和添加元素时，速度慢。

LinkedList：提供优化的顺序访问性能，同时可以高效地子列表中部插入和删除操作。但随即访问时，速度慢。

### SETs

Set拥有与Collection完全相同的接口，一个Set只允许每个对象存在一个实例

HashSet：用于除非常小的以外的所有Set。对象也必须定义hashCode()。

ArraySet：面向非常小的Set 设计，特别是那些需要频繁创建和删除的。对于小Set，与HashSet 相比，ArraySet 创建和反复所需付出的代价都要小得多。但随着Set 的增大，它的性能也会大打折扣。不需要HashCode()。

TreeSet：由一个“红黑树”后推得到的顺序Set。

### Map

Map（接口） 维持“键－值”对应关系（对），以便通过一个键查找相应的值

HashMap：基于一个散列表实现。针对“键值对”的插入和检索，这种形式具有最稳定的性能。

ArrayMap：有一个ArrayList后推得到Map。对反复的顺序提供了精确的控制。面向非常小的Map 设计，特别是那些需要经常创建和删除的。对于非常小的Map，创建和反复所付出的代价要比HashMap 低得多。但在Map 变大以后，性能也会相应地大幅度降低。

TreeMap：在一个“红－黑”树的基础上实现。查看键或者“键－值”对时，它们会按固定的顺序排列（取决于Comparable 或Comparator，稍后即会讲到）。TreeMap 最大的好处就是我们得到的是已排好序的结果。TreeMap 是含有subMap()方法的唯一一种Map，利用它可以返回树的一部分。

选择List：

[![QQ截图20150610162659](https://napuzhang.files.wordpress.com/2015/06/qqe688aae59bbe20150610162659.png?w=300)](https://napuzhang.files.wordpress.com/2015/06/qqe688aae59bbe20150610162659.png)

&nbsp;

选择Set：

[![QQ截图20150610162829](https://napuzhang.files.wordpress.com/2015/06/qqe688aae59bbe20150610162829.png?w=300)](https://napuzhang.files.wordpress.com/2015/06/qqe688aae59bbe20150610162829.png)

&nbsp;

选择Map：

[![QQ截图20150610162957](https://napuzhang.files.wordpress.com/2015/06/qqe688aae59bbe20150610162957.png?w=300)](https://napuzhang.files.wordpress.com/2015/06/qqe688aae59bbe20150610162957.png)

&nbsp;

(1) 数组包含了对象的数字化索引。它容纳的是一种已知类型的对象，所以在查找一个对象时，不必对结果进行造型处理。数组可以是多维的，而且能够容纳基本数据类型。但是，一旦把它创建好以后，大小便不能变化了。
(2) Vector（矢量）也包含了对象的数字索引——可将数组和Vector 想象成随机访问集合。当我们加入更多的元素时，Vector 能够自动改变自身的大小。但Vector 只能容纳对象的句柄，所以它不可包含基本数据类型；而且将一个对象句柄从集合中取出来的时候，必须对结果进行造型处理。
(3) Hashtable（散列表）属于Dictionary（字典）的一种类型，是一种将对象（而不是数字）同其他对象关联到一起的方式。散列表也支持对对象的随机访问，事实上，它的整个设计方案都在突出访问的“高速度”。
(4) Stack（堆栈）是一种“后入先出”（LIFO）的队列。