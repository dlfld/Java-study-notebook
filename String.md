# String类

**在 Java 8 中，String 内部使用 char 数组存储数据。在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。**

+ 字符使用的是unicode编码，一个字符（不区分字母还是汉字）占两个字节。
+ String 有很多的构造器
+ String实现了Serializable接口 和Comparable接口。可以串行化和可以比较
+ String是final的。
+ String有属性 private final char value[] 用于存放字符串内容，因此string 底层就是一个字符串数组
  + final 类型的，被赋值之后是**不可以修改**的 ，不可修改指的是value不能够**指向新的地址**，但是单个字符的内容是可以改变的

## String 不可变（final）的好处

1. 可以缓存hash值

   因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

2. String Pool 的需要

   如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

3. 安全性

   String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 的那一方以为现在连接的是其它主机，而实际情况却不一定是。

4. 线程安全

   String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

## 两种创建String对象的区别

方式一：String a  = “sss”

方式二： String a = new String(“aaa”)

1. 方式一：先从常量池查看是否有“sss”的数据空间，如果有就直接指向。如果没有就重新创建，然后指向。a最终指向的是常量池的空间地址。
2. 方式二：现在堆空间中创建，里面维护了value属性，指向常量池的“sss”空间。如果常量池没有“sss”，重新创建，再让value指向，如果有，直接通过value指向。最终指向的是堆中的空间地址。

![image-20211103171118760](%E5%9B%BE%E7%89%87%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8/image-20211103171118760.png)

#### String 类测试题

```java
String a = "aaa";
String b = new String("aaa");
/**
 * itern()
 *
 * 当调用itern方法的时候，如果常量池中有与当前字符串值相等的字符串就返回那个字符串的地址，
 * 如果没有的话就把它添加到常量池中并返回地址
 */
System.out.println(a == b.intern()); //true
//b对象指向堆的value的地址，而b.intern()指向的是常量池中字符串的地址，所以不相等 
System.out.println(b == b.intern()); //false 
```



```java
/**
 * 此时系统创建了两个对象
 */
String x = "hello";
x = "hello ";
/**
 * 此时创建了几个对象？ 一个对象
 * 在编译的时候会把hello和abc直接合并起来作为一个对象
 * 优化等于"hellohaha"  
 */
String aa = "hello" + "haha";

/**
 * 创建了几个对象？
 *
 *一共三个对象 "hello"  "abc"  "helloabc"
 */
String a3 = "hello";
String b3 = "abc";
//1. 先创建一个StringBuilder对象
//2. 执行sb.append("hello")
//3. 执行sb.append("abc")
//4. 执行String c = sb.toString()方法
//最后是c指向堆中的对象（string） value[] 然后这个value指向池中的对象"helloabc"
String c3 = a3 + b3; //指向堆中的地址 因为是一个StringBuilder对象
String c4 = "helloabc";
System.out.println(c3==c4); //false
```



# StringBuffer

很多方法都是和String相同的，但是StringBuffer是可变长度的

+ StringBuffer 的直接父类是AbstractStringBuilder
+ Stringbuffer 实现了序列化接口，可以串形化。
+ 在父类中 AbstractStringBuilder 有属性 char[] value 不是final 
+ 改value数组存放字符串内容，因此存放在**堆里**
+ StringBuffer是一个finall类，不能被继承

## String 和StringBuffer的对比

+ String 保存的是字符串常量，里面的值不能够更改，每次String类的更新实际上就是更改地址，效率比较低。
+ StringBuffer保存的是字符串变量，里面的值可以更改，每次StringBuffer的更新实际上可以更新内容，不用每次更新地址，效率较高 
+ 不用每次都更换地址，不用每次创建新的对象，因此效率是高于String的

### StringBuffer类

+ 初始化为16个char 数组
+ 通过构造器指定char数组的大小
+ 如果构造器传入的是一个字符串则char数组长度为字符串长度+16

## StringBuilder

是一个可变的字符序列，此类提供一个与StringBuffer兼容的API，但不保证同步（不是线程安全的）。该类被设计用作StringBuffer的一个简易替换，**用在字符串缓冲区被单个线程使用的时候。**如果可能，简易优先采用该类，因为在大多数实现中，它比StringBuffer要快。

StringBuffer因为是线程安全的，所以加了锁，加锁之后速度就慢了，因此在单线程使用的时候加锁的时间就是非必要的。所以就用StringBuilder



### String, StringBuffer and StringBuilder

**1. 可变性**

- String 不可变 
- StringBuffer 和 StringBuilder 可变

**2. 线程安全**

- String 不可变，因此是线程安全的
- StringBuilder 不是线程安全的
- StringBuffer 是线程安全的，内部使用 synchronized 进行同步，基本上内部大部分的方法都用了**synchronized**关键字

3. 效率
   + String Builder > String Buffer > String

## 使用的原则和结论

1. 如果字符串存在大量的修改操作，一般使用StringBuffer或者是StringBuilder
2. 如果字符串存在大量的修改操作，并在单线程的情况，使用StringBuilder
3. 如果字符串存在大量的修改操作，并在多线程的情况，使用StringBuffer
4. 如果我们的字符串很少修改，被多个对象引用，使用String，比如配置信息等。 