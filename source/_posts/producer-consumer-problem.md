title: Producer Consumer Problem
tags:
  - concurrent
  - java
  - multiThread
id: 234
categories:
  - Java
date: 2015-06-15 07:44:08
---

### 问题描述

假设我们有两个进程或者线程。他们是工厂里一条流水线上的两个车间。其中线程A生产的部件要交给下一个线程B来做进一步的处理。这个时候，线程A就相当于一个生产者，而线程B就相当于一个消费者。在生产者产生了一个不见后，它们会通过一个传送带传递这个部件。也就是说，线程A负责传送带上面放部件，而线程B负责在传送带上面取部件。这样，两个线程在各自运行的时候需要访问同一个资源。

### 解决方法

我们可以将两个线程之间要共同访问的部分定义为一个数组。那么，对于两个线程来说，他们在一定程度上是独立的。一个线程可以向共享资源队列里面放东西，一个线程从里面取东西。对于Producer线程来说，它需要考虑的就是如果我往资源队列里放东西时，队列满了，那么我必须要等待consumer线程取走一部分元素，这样才能继续进行。另外，和其他线程访问同一个资源队列的时候，还要保证访问时线程安全的。同样，对于consumer线程来说，如果资源队列里面是空的，那么就必须等待producer线程将产生的元素放入队列。

我们的问题核心就是在于怎样使得一个线程和另外一个线程互斥的访问同一资源呢？另外，如果一个线程发现目前的资源状况不适合自己，又该怎样停止自己而让其他线程来运行呢？

#### （1）wait notify

在java里面。如果我们希望一个线程在已经占有资源锁的情况下先中止自己的执行并释放锁，那么就需要wait。如果我们已经完成自己那部分的操作，需要释放锁并要其他的线程来继续执行，我们通过调用notify和notifyAll方法来通知其他原因资源被枷锁之后处于阻塞状态的线程。

一个producer的执行过程如下：

1.  如果资源队列满了，则调用wait方法释放锁，一直等待到资源队列有空缺。
2.  在资源队列里面加入新的元素。
3.  调用notify和notifyAll方法来唤醒其他等待的线程。
<pre>import java.util.Date;
import java.util.LinkedList;

public class EventStorage {
 private int maxSize;
 private LinkedList&lt;Date&gt; storage;

 public EventStorage() {
 maxSize = 10;
 storage = new LinkedList&lt;&gt;();
 }

 public synchronized void set() {
 while(storage.size() == maxSize) {
 try {
 wait();
 } catch (InterruptedException e) {
 e.printStackTrace();
 }
 }
 storage.offer(new Date());
 System.out.printf("Set: %d", storage.size());
 notifyAll();
 }

 public synchronized void get() {
 while(storage.size() == 0) {
 try {
 wait();
 } catch(InterruptedException e) {
 e.printStackTrace();
 }
 }
 System.out.printf("Get: %d: %s", storage.size(), storage.poll());
 notifyAll();
 }
}

public class Consumer implements Runnable {

 /**
 * Store to work with
 */
 private EventStorage storage;

 /**
 * Constructor of the class. Initialize the storage
 * @param storage The store to work with
 */
 public Consumer(EventStorage storage){
 this.storage=storage;
 }

 /**
 * Core method for the consumer. Consume 100 events
 */
 @Override
 public void run() {
 for (int i=0; i&lt;100; i++){
 storage.get();
 }
 }

}

public class Producer implements Runnable {

 /**
 * Store to work with
 */
 private EventStorage storage;

 /**
 * Constructor of the class. Initialize the storage.
 * @param storage The store to work with
 */
 public Producer(EventStorage storage){
 this.storage=storage;
 }

 /**
 * Core method of the producer. Generates 100 events.
 */
 @Override
 public void run() {
 for (int i=0; i&lt;100; i++){
 storage.set();
 }
 }
}

public class Main {

 /**
 * Main method of the class. Create and star two initialization tasks
 * and wait for their finish
 * @param args
 */
 public static void main(String[] args) {
 EventStorage storage = new EventStorage();

 Producer producer = new Producer(storage);
 Thread thread1 = new Thread(producer);

 Consumer consumer = new Consumer(storage);
 Thread thread2 = new Thread(consumer);

 thread2.start();
 thread1.start();
 }
}</pre>
synchronized对于线程访问，每次只能有一个线程能够获得这个锁进入这个方法。使用wait方法，能够在获得这个锁的情况下释放锁。

### BlockingQueue

Java中有一些数据结构包含有互斥锁，BlockingQueue定义了一个借口，具体实现包括：ArrayBlockingQueue，LinkedBlockingQueue.
<pre> import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ArrayBlockingQueue;

class Producer implements Runnable{

 private BlockingQueue&lt;Message&gt; queue;

 public Producer(BlockingQueue&lt;Message&gt; q){

 this.queue = q;
 }

 @Override
 public void run(){

 for(int i=0; i&lt;100; i++){

 Message msg = new Message(""+i);
 try{
 Thread.sleep(i);
 queue.put(msg);
 System.out.println("Produced "+msg.getMsg());
 }catch(InterruptedException e){
 e.printStackTrace();
 }
 }

 Message msg = new Message("exit");
 try{
 queue.put(msg);
 }catch(InterruptedExcetpion e){
 e.printStackTrace();
 }
 }
}

class Consumer implements Runnable{

 private BlockingQueue&lt;Message&gt; queue;
 public Consumer(BlockingQueue&lt;Message&gt; q){

 this.queue = q;
 }

 @Override
 public void run(){

 try{
 Message msg;
 while((msg = queue.take()).getMsg()!="exit"){

 Thread.sleep(i);
 System.out.println("Consumed "+msg.getMsg());
 }
 }catch(InterruptedException e){
 e.printStackTrace();
 }
 }
}

public class ProducerConsumerService{
 public static void main(String args[]){

 BlockingQueue&lt;Message&gt; queue = new ArrayBlockingQueue&lt;&gt;(10);
 Producer producer = new Producer(queue);
 Consumer consumer = new Consumer(queue);

 new Thread(producer).start();
 new Thread(consumer).start();

 System.out.println("Producer and consumer has been started!");
 }

}</pre>