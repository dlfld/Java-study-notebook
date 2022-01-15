# BlockingQueue

##  什么是BlockingQueue?

`BlockingQueue`其实就是阻塞队列，是基于阻塞机制实现的线程安全的队列。而阻塞机制的实现是通过在入队和出队时加锁的方式避免并发操作。

`BlockingQueue`不同于普通的`Queue`的区别主要是：

1. 通过在入队和出队时进行加锁，保证了队列线程安全
2. 支持阻塞的入队和出队方法：当队列满时，会阻塞入队的线程，直到队列不满；当队列为空时，会阻塞出队的线程，直到队列中有元素。

`BlockingQueue`常用于**生产者-消费者模型**中，往队列里添加元素的是生产者，从队列中获取元素的是消费者；通常情况下生产者和消费者都是由多个线程组成；下图所示则为一个最常见的**生产者-消费者模型**，生产者和消费者之间通过队列平衡两者的的处理能力、进行解耦等。

![img](BlockingQueue.assets/56a643d303214e32a4e5d4b7bc186a17~tplv-k3u1fbpfcp-watermark.awebp)

##  BlockingQueue接口定义

`BlockingQueue`继承了`Queue`接口，在Queue接口基础上，又提供了若干其他方法，其定义源码如下：(JDK17)

```java
ublic interface BlockingDeque<E> extends BlockingQueue<E>, Deque<E> {

    void addFirst(E e);


    void addLast(E e);


    boolean offerFirst(E e);


    boolean offerLast(E e);


    void putFirst(E e) throws InterruptedException;


    void putLast(E e) throws InterruptedException;

    boolean offerFirst(E e, long timeout, TimeUnit unit)
        throws InterruptedException;

    boolean offerLast(E e, long timeout, TimeUnit unit)
        throws InterruptedException;

    E takeFirst() throws InterruptedException;

    E takeLast() throws InterruptedException;

    E pollFirst(long timeout, TimeUnit unit)
        throws InterruptedException;

    E pollLast(long timeout, TimeUnit unit)
        throws InterruptedException;

    boolean removeFirstOccurrence(Object o);

    boolean removeLastOccurrence(Object o);

    boolean add(E e);

    boolean offer(E e);

    void put(E e) throws InterruptedException;

    boolean offer(E e, long timeout, TimeUnit unit)
        throws InterruptedException;

    E remove();
 
    E poll();

    E take() throws InterruptedException;

    E poll(long timeout, TimeUnit unit)
        throws InterruptedException;

    E element();

    E peek();

    boolean remove(Object o);

    boolean contains(Object o);

    int size();

    Iterator<E> iterator();
    void push(E e);
}
```

`BlockingQueue`主要提供了四类方法，如下表所示：

| 方法         | 抛出异常    | 返回特定值 | 阻塞     | 阻塞特定时间           |
| ------------ | ----------- | ---------- | -------- | ---------------------- |
| 入队         | `add(e)`    | `offer(e)` | `put(e)` | `offer(e, time, unit)` |
| 出队         | `remove()`  | `poll()`   | `take()` | `poll(time, unit)`     |
| 获取队首元素 | `element()` | `peek()`   | 不支持   | 不支持                 |

除了**抛出异常**和**返回特定值**方法与Queue接口定义相同外，BlockingQueue还提供了两类阻塞方法：一种是当队列没有空间/元素时一直阻塞，直到有空间/有元素；另一种是在特定的时间尝试入队/出队，等待时间可以自定义。

在本文开始我们了解到，BlockingQueue是线程安全的队列，所以提供的方法也都是线程安全的；那么下面我们就继续看下BlockingQueue的实现类，以及如何实现线程安全和阻塞。

## BlockingQueue实现类及原理

### 主要实现类

BlockingQueue接口主要由5个实现类，分别如下表所示。

| 实现类                      | 功能                                                         |
| --------------------------- | ------------------------------------------------------------ |
| **`ArrayBlockingQueue`**    | **基于数组的阻塞队列**，使用数组存储数据，并需要指定其长度，所以是一个**有界队列** |
| **`LinkedBlockingQueue`**   | **基于链表的阻塞队列**，使用链表存储数据，默认是一个**无界队列**；也可以通过构造方法中的`capacity`设置最大元素数量，所以也可以作为**有界队列** |
| **`SynchronousQueue`**      | 一种没有缓冲的队列，生产者产生的数据直接会被消费者获取并且立刻消费 |
| **`PriorityBlockingQueue`** | 基于**优先级别的阻塞队列**，底层基于数组实现，是一个**无界队列** |
| **`DelayQueue`**            | **延迟队列**，其中的元素只有到了其指定的延迟时间，才能够从队列中出队 |

其中在日常开发中用的比较多的是`ArrayBlockingQueue`和`LinkedBlockingQueue`，本文也将主要介绍这两个实现类的原理。



作者：毛与帆
链接：https://juejin.cn/post/6999798721269465102
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。