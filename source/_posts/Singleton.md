title: Singleton
date: 2015-12-28 09:52:28
tags: Java-Singleton
---

## 目的
确定某个类只有一个实例，并且为之提供一个全局访问点，为了防止其他工作人员实例化我们的类。

## 方法
为该类创建唯一一个构造器，并将构造器设置为私有。不能有有其他非私有构造器，或者根本又有为该类提供构造器，其他人员任然能够实例化我们的类。

## java里面通常有两种方法实现
* 饿汉模式： 指全局的单例实例在类装载时构建，急切初始化。
* 懒汉模式： 指全局的单例实例在第一次被使用时构建，延迟初始化。

## 饿汉模式
采用饿汉模式可以避免线程安全问题，但是任何对Singleton类的访问，比如类中有另外一个static方法被访问，将会引起jvm去初始化instance，而此时我们的本意是不想加载单例类。同时又因为没有延迟加载，最明显的缺点就是如果构造器内的方法比较耗时，则加载过程会比较长。对于一般的应用，构造方法内的代码不涉及到读取配置，远程调用，初始化IOC容器等长时间执行的情况，这种方法最简单。
```
public class Singleton{
  private Singleton(){}
  private static Singleton instance = new Singleton();
  public static Singleton getInstance(){
    return instance;
  }
}
```
## 懒汉模式
单线程下没有问题，但多线程情况下可能会出现两个或以上的instance实例的情况。
```
public class Singleton{
  private Singleton(){}
  private static Singleton instance = null;
  public static Singleton getInstance(){
    if(instance==null){
      instance = new Singleton();
    }
    return instance;
  }
}
```
问题：线程1在判断instance=null为真，执行new 操作之前，判断为真之后，线程2正好执行判断操作，这是instance还是null。因此线程2也会执行new操作。
改进代码
```
public class Singleton{
  private Singleton(){}
  private static Singleton instance = null;
  public static synchronized Singleton getInstance(){
    if(instance == null){
      instance = new Singleton();
    }
    return instance;
  }
}
```
getInstance 会被外部线程比较频繁的调用，同步比非同步要消耗更多昂贵的资源，因此这样的做法存在很大的额外性能消耗。因此产生如下双检查写法：
```
public class Singleton{
  private Singleton(){}
    private static Singleton instance = null;
    public static Singleton getInstance(){
      if(instance == null){
        synchronized(Singleton.class){
          if(instance == null){
            instance == new Singleton();
          }
        }
      }
      return instance;
    }
}
```
instance = new Singleton(), 对于编译器来说，分两步走，首先初始化一个instance 对象并随意赋一个值，然后调用Singleton类的构造器赋值。因此在第一步完成之后instance==null这一步已经不成立了,但此时instance只是一个临时值。如果线程2此时进入getInstance,就会把这个instance给返回。
可以用一个容器类来解决这个问题。即可以使用延迟加载，让类在被使用的时候才加载，又避免额外的同步调用开销，同时还不使用双检查的模式。
```
public class Singleton{
  private static class SingletonHolder{
    static Singleton instance = new Singleton();
  }
  public static Singleton getInstance(){
    return SingletonHolder.instance;
  }
  private Signleton(){}
}
```
java中，只有一个类被用到的时候才被初始化。在getInstance方法被调用的时候，如果SingletonHolder类没有被加载，就会去加载，起到延迟加载的作用，同时也能保持多线程下的语义正确性。
