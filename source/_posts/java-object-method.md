title: Java Object Method
id: 200
categories:
  - Java
date: 2015-05-14 08:06:34
tags:
---

Java.lang.Object

java.lang 包在使用的时候无需显示导入，编译时由编译器自动导入。

Object类是类层次结构的根，Java类所有的类从根本上都是继承于这个类

Object是唯一没有父类的类

Object 类中的方法

![](http://images.cnitblog.com/blog/325852/201301/03120453-53ad86e99a464684ab5e89fe6c4089e1.png)

1，getClass

public final Class&lt;?extends Object&gt; getClass()

返回一个对象的运行时类。该Class对象是由所表示类的static synchronized方法锁定的对象。

2，hashCode

public int hashCode()

返回该对象的哈希码值。支持该方法是为哈希表提供一些优点，例如：java.util.Hashtable提供的哈希表。

`hashCode` 的常规协定是：

*   在 Java 应用程序执行期间，在同一对象上多次调用 <tt>hashCode</tt> 方法时，必须一致地返回相同的整数，前提是对象上 <tt>equals</tt> 比较中所用的信息没有被修改。从某一应用程序的一次执行到同一应用程序的另一次执行，该整数无需保持一致。
*   如果根据 <tt>equals(Object)</tt> 方法，两个对象是相等的，那么在两个对象中的每个对象上调用 `hashCode` 方法都必须生成相同的整数结果。
*   以下情况_不_ 是必需的：如果根据 [`equals(java.lang.Object)`](/java%20tools/jdk1.5%E5%B8%AE%E5%8A%A9%E6%96%87%E6%A1%A3/java/lang/Object.html#equals(java.lang.Object)) 方法，两个对象不相等，那么在两个对象中的任一对象上调用 <tt>hashCode</tt> 方法必定会生成不同的整数结果。但是，程序员应该知道，为不相等的对象生成不同整数结果可以提高哈希表的性能。
实际上，由 <tt>Object</tt> 类定义的 hashCode 方法确实会针对不同的对象返回不同的整数。（这一般是通过将该对象的内部地址转换成一个整数来实现的，但是 Java<sup>TM</sup> 编程语言不需要这种实现技巧。）

3，equals

public boolean equals(Object obj)

判断某个对象是否与当前对象相等。

`equals` 方法在非空对象引用上实现相等关系：

*   _自反性_：对于任何非空引用值 `x`，`x.equals(x)` 都应返回 `true`。
*   _对称性_：对于任何非空引用值 `x` 和 `y`，当且仅当 `y.equals(x)` 返回 `true` 时，`x.equals(y)` 才应返回 `true`。
*   _传递性_：对于任何非空引用值 `x`、`y` 和 `z`，如果 `x.equals(y)` 返回 `true`，并且 `y.equals(z)` 返回 `true`，那么 `x.equals(z)` 应返回 `true`。
*   _一致性_：对于任何非空引用值 `x` 和 `y`，多次调用 <tt>x.equals(y)</tt> 始终返回 `true` 或始终返回 `false`，前提是对象上 `equals` 比较中所用的信息没有被修改。
*   对于任何非空引用值 `x`，`x.equals(null)` 都应返回 `false`。
`Object` 类的 <tt>equals</tt> 方法实现对象上差别可能性最大的相等关系；即，对于任何非空引用值 `x` 和 `y`，当且仅当 `x` 和 `y` 引用同一个对象时，此方法才返回 `true`（`x == y` 具有值`true`）。

注意：当此方法被重写时，通常有必要重写 <tt>hashCode</tt> 方法，以维护 <tt>hashCode</tt> 方法的常规协定，该协定声明相等对象必须具有相等的哈希码。

<dl><dt>**参数：**</dt><dd>`obj` - 要与之比较的引用对象。</dd><dt>**返回：**</dt><dd>如果此对象与 obj 参数相同，则返回 `true`；否则返回 `false`。</dd></dl>

4，clone

protected Object clone() throws CloneNotSupportedExcetption

创建并返回此对象的一个副本。

<tt>Object</tt> 类的 <tt>clone</tt> 方法执行特定的克隆操作。首先，如果此对象的类不能实现接口 <tt>Cloneable</tt>，则会抛出 <tt>CloneNotSupportedException</tt>。注意：所有的数组都被视为实现接口<tt>Cloneable</tt>。否则，此方法会创建此对象的类的一个新实例，并像通过分配那样，严格使用此对象相应字段的内容初始化该对象的所有字段；这些字段的内容没有被自我克隆。所以，此方法执行的是该对象的“浅表复制”，而不“深层复制”操作。

<tt>Object</tt> 类本身不实现接口 <tt>Cloneable</tt>，所以在类为 <tt>Object</tt> 的对象上调用 <tt>clone</tt> 方法将会导致在运行时抛出异常。

<dl><dt>**返回：**</dt><dd>此实例的一个克隆。</dd><dt>**抛出：**</dt><dd>`[CloneNotSupportedException](/java%20tools/jdk1.5%E5%B8%AE%E5%8A%A9%E6%96%87%E6%A1%A3/java/lang/CloneNotSupportedException.html "java.lang 中的类")` - 如果对象的类不支持 `Cloneable` 接口，则重写 `clone` 方法的子类也会抛出此异常，以指示无法克隆某个实例。</dd></dl>

5，toString

public String toString()

return getClass().getName()+"@"+Integer.toHexString(hashCode())

6，notify

public final void notify()

唤醒在此对象监视器上等待的单个线程。如果所有线程都在此对象上等待，则会选择唤醒其中一个线程。选择是任意性的，并在对实现做出决定时发生。线程通过调用其中一个`wait` 方法，在对象的监视器上等待。

7，notifyAll

public final void notifAll()

唤醒在此对象监视器上等待的所有线程。线程通过调用其中一个 `wait` 方法，在对象的监视器上等待。

8，finalize

protected void finalize() throws Throwable

当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。子类重写 `finalize` 方法，以配置系统资源或执行其他清除。

<tt>finalize</tt> 的常规协定是：当 Java<sup>TM</sup> 虚拟机已确定尚未终止的任何线程无法再通过任何方法访问此对象时，将调用此方法，除非由于准备终止的其他某个对象或类的终结操作执行了某个操作。<tt>finalize</tt> 方法可以采取任何操作，其中包括再次使此对象对其他线程可用；不过，<tt>finalize</tt> 的主要目的是在不可撤消地丢弃对象之前执行清除操作。例如，表示输入/输出连接的对象的 finalize 方法可执行显式 I/O 事务，以便在永久丢弃对象之前中断连接。

<tt>Object</tt> 类的 <tt>finalize</tt> 方法执行非特殊性操作；它仅执行一些常规返回。<tt>Object</tt> 的子类可以重写此定义。

Java 编程语言不保证哪个线程将调用某个给定对象的 <tt>finalize</tt> 方法。但可以保证在调用 finalize 时，调用 finalize 的线程将不会持有任何用户可见的同步锁定。如果 finalize 方法抛出未捕获的异常，那么该异常将被忽略，并且该对象的终结操作将终止。

在启用某个对象的 <tt>finalize</tt> 方法后，将不会执行进一步操作，直到 Java 虚拟机再次确定尚未终止的任何线程无法再通过任何方法访问此对象，其中包括由准备终止的其他对象或类执行的可能操作，在执行该操作时，对象可能被丢弃。

对于任何给定对象，Java 虚拟机最多只调用一次 <tt>finalize</tt> 方法。

`finalize` 方法抛出的任何异常都会导致此对象的终结操作停止，但可以通过其他方法忽略它。

<dl><dt>**抛出：**</dt><dd>`[Throwable](/java%20tools/jdk1.5%E5%B8%AE%E5%8A%A9%E6%96%87%E6%A1%A3/java/lang/Throwable.html "java.lang 中的类")` - 此方法抛出的 `Exception`</dd><dd></dd></dl>

9，wait

public final void wait(long time) throws InterruptedException

导致当前的线程等待，直到其他线程调用此对象的 `notify()` 方法或 `notifyAll()` 方法，或者超过指定的时间量。