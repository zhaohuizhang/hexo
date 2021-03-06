title: Abstract 与 Interface的区别
tags:
  - Abstract
  - Interface
id: 190
categories:
  - Java
date: 2015-05-13 02:43:38
---

1，abstract class 在Java语言中表示一种继承关系，一个类只能使用一次继承关系。但是一个类可以有多个实现的interface。

2，在Abstract class中可以有自己的数据成员，也可以有非abstract的成员方法，而在interface中，只能够有静态的不能被修改的数据成员（必须是static final，不过在interface里面一般不定义数据成员），所有的成员方法都是abstract的。

3，abstract class和interface所反映出的设计理念不同，其中abstract class表示的是“is - a”关系，interface表示的是“like - a”。

4，实现抽象类和接口的类必须实现其中的所有方法。抽象类中可以有非抽象方法。接口中则不能实现方法。

5，接口中定义的变量默认是public static final 型，且必须给其初值，所以实现类中不能重新定义，也不能改变变量。

6，抽象类中的变量默认是friendly型，其值可以再子类中重新定义，也可以重新赋值。

7，接口中的方法默认都是 public abstract 类型的。

总结：

*   接口体现的是一种设计规范，抽象类体现的是模板式设计。
*   接口里的方法必须是public abstract抽象方法，抽象类里可以有方法实现。
*   接口里不可以定义静态方法，抽象类可以。
*   接口里的变量全部为静态常量，抽象类里可有普通变量。
*   接口里不可以有构造函数和初始化块，抽象类里可以有。
*   一个类可以实现多个接口，但只能集成一个抽象类。
接口是一种非常有效的编程方法，它让对象的定义与实现分离，从而可以在不破坏现有应用程序的情况下使对象得以完善与进化。接口消除了实现继承的一个大问题，就是在对设 计实施后再对其进行更改时很可能对代码产生破坏。即使实现继承允许类从基类继承实现，在类首次发布时仍然会使我们不得不为设计做很多的抉择。如果原有的设想不正确，并非 总可以在以后的版本中对代码进行安全的更改。

接口一旦被定义和接受，就必须保持不变，以保护为使用该接口而编写的应用程序。接口发布后，就不能对其进行更改。这是我们进行组件设计的一个重要原则，叫做‘接口不变性’。