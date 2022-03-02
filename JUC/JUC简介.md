# JUC简介



## 线程状态枚举类

Thread.State

| 状态          | 解释     |
| ------------- | -------- |
| NEW           | 新建     |
| RUNABLE       | 准备就绪 |
| BLOCKED       | 阻塞     |
| WAITING       | 不见不散 |
| TIMED_WAITING | 过时不候 |
| TERMINATED    | 终结     |

## wai/sleep的区别

sleep 是Thread的静态方法，wait是Object方法，任何对象实例都能调用。

sleep不会释放锁，他也不需要占用锁。wait会释放锁，但调用它的前提是当前线程占有锁（即代码要在synchronized中）。

## 管程

是一种监视器，一种同步机制，保证在同一个时间，只有一个线程访问被保护数据或者代码，jvm同步给予进入和退出，使用管程对像实现的。 线程进入退出需要加锁，加锁操作通过管程对象进行操作。

## 线程

线程分为用户线程和守护线程。

用户线程：用户自定义线程。主线程结束了，用户线程还没结束，jvm存活

守护线程：是一种特殊线程，运行在后台。比如垃圾回收。 没有用户线程，都是守护线程jvm就会结束

设置一个线程是用户线程还是守护线程的时候，应该在start之前 设置。

```java
public static void main(String[] args) {
    Thread thread = new Thread(() -> {
        //isDaemon 为true表示是守护线程 为false表示是用户线程
        System.out.println("进入了线程" + Thread.currentThread().getName() + "::" + Thread.currentThread().isDaemon());
    });
    //设置线程为守护线程
    thread.setDaemon(true);
    thread.start();

}
```



## Lock与synchronized的区别

+ Lock不是java语言内置的，synchronized是Java语言的关键字，因此是内置特性。Lock是一个类，通过这个类可以实现同步访问。
+ Lock和synchronized有一点非常大的不同，采用synchronized不需要用户去手动释放锁，当synchronized方法或者synchronized代码块执行完之后，系统会自动让线程释放对锁的占用；而Llock则必须要去用户手动释放锁，如果没有主动释放锁，就可能导致死锁现象。

## Lock接口

Lock锁实现提供了比使用同步方法和语句可以获得更广泛的锁操作。他们允许更灵活的结构，可能具有非常不同的属性，并且可能支持多个关联的条件对象。Lock提供了比synchronized更多的功能。

Lock接口的实现类<code>ReentrantLock</code>,<code>ReentrantReadWriteLock.ReadLock</code>,<code>ReentrantReadWriteLock.WriteLock</code>

ReentrantLock:可重入锁，