# JUC简介



## 线程状态枚举类

Thread.State

在调用线程的start方法的时候，线程不一定会马上创建，start方法内的start是nativa的，靠的是操作系统，因此就不是java能够控制的而是操作系统控制的。

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

Lock 锁

```java
class LTicket {
    private int number = 30;
    //创建可重入锁
    private final ReentrantLock lock = new ReentrantLock();

    //买票方法
    public void sale() {
        //上锁
        lock.lock();
        try {
            if (number > 0) {
                System.out.println(Thread.currentThread().getName() + "卖出" + (number--) + "剩余" + number);
            }
        } finally {
            //解锁 解锁写在finally中
            lock.unlock();
        }
    }
}

public class LSaleTicket {


    public static void main(String[] args) {
        LTicket ticket = new LTicket();
        new Thread(() -> {
            for (int i = 0; i < 3; i++) {
                ticket.sale();
            }
        }, "aa").start();

        new Thread(() -> {
            for (int i = 0; i < 3; i++) {
                ticket.sale();
            }
        }, "bb").start();


        new Thread(() -> {
            for (int i = 0; i < 3; i++) {
                ticket.sale();
            }
        }, "cc").start();
    }
}
```



多线程的通信

设计两个方法对一个变量进行操作，一个方法是对变量执行+1操作，另一个方法是对变量进行-1操作，当变量执行+1操作时候如果变量为1就等待，当变量为0的时候就加一并唤醒正在等待-1的线程

```java
class Share {
    private int number = 0;

    //    +1
    public synchronized void incr() throws InterruptedException {
        //如果number不是0 那就等待  当他为0的时候再加一
        if (number != 0) {
            this.wait();
        }
        //如果number的值为0 就加一
        number++;
        System.out.println(Thread.currentThread().getName() + "::" + number);
        //通知其他线程
        this.notifyAll();
    }

    public synchronized void decr() throws InterruptedException {
        if (number != 1) {
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName() + "::" + number);
        this.notifyAll();
    }
}

public class ThreadDemo1 {
    //    创建多个线程调用资源类的操作线程
    public static void main(String[] args) {
        Share share = new Share();
        new Thread(() -> {
            for (int i = 0; i < 11; i++) {
                try {
                    share.incr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "aa").start();

        new Thread(() -> {
            for (int i = 0; i < 11; i++) {
                try {
                    share.decr();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "bb").start();
    }
}
```