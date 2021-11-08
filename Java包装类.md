# Java自动装箱和自动拆箱
## 自动装箱和自动拆箱的定义
	在java中所有的类都是对象,但是有八种基本数据类型是例外.这八种基本数据类型 byte ,short,int,long,char,float,double,boolean 都不具备对象的特性,即不携带属性,没有方法可以调用.为了解决这个问题,javaJava为每种基本数据类型分别设计了对应的类，称之为包装类(Wrapper Classes).
	Java有基本类型和包装类型之说，基本类型就是byte、int这一类的，包装类型就是Byte、Integer这一类的，自动装箱就是把一个int的值赋值给Integer。自动拆箱就是把Integer的值赋值给int


```java
//自动装箱
Integer a = 123;
//自动拆箱
int b = a;
```
具体的基本类型为：
| 基本数据类型 | 包装类    |
| ------------ | --------- |
| int          | Integer   |
| byte         | Byte      |
| short        | Short     |
| long         | Long      |
| char         | Character |
| float        | Float     |
| double       | Double    |
| boolean      | Boolean   |


## 实际原理
通过以下代码可以看出他是返回了一个Integer的对象来替代int，改方法会缓存-128～127之间的值，如果值超过了的话就返回一个新的对象。如果没有超过的话就返回缓存中的值
IntegerCache.cache是一个数组，里面有对应的值
IntegerCache是Integer类中的静态内部类，用于缓存数据便于节省内存、提高性能。
```java
//这是jdk源码中创建cache数组的代码
               Integer[] c = new Integer[size];
                int j = low;
                for(int i = 0; i < c.length; i++) {
                    c[i] = new Integer(j++);
                }
```

```java

    /**
     * Returns an {@code Integer} instance representing the specified
     * {@code int} value.  If a new {@code Integer} instance is not
     * required, this method should generally be used in preference to
     * the constructor {@link #Integer(int)}, as this method is likely
     * to yield significantly better space and time performance by
     * caching frequently requested values.
     *
     * This method will always cache values in the range -128 to 127,
     * inclusive, and may cache other values outside of this range.
     *
     * @param  i an {@code int} value.
     * @return an {@code Integer} instance representing {@code i}.
     * @since  1.5
     */
    @IntrinsicCandidate
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```
那么是不是所有的包装类型都会将数值相同的对象指向同一个内存空间吗?
事实上,只有Integer、Short、Byte、Character、Long这几个类的valueOf才会如此.
而Double、Float的valueOf则会为每个对象分配不同的内存空间

### 除了Integer之外，在其他包装类(例如：Byte，Short，Long等)中也存在类似的设计。



## Integer 类面试题总结

只要有基本数据类型，== 判断的就是值是否相等

```java
//只要有基本数据类型，== 判断的就是值是否相等
Integer a = 127;
int b = 127;
System.out.println(a==b);  //true
Integer i9 = 128; 
Integer i10 = new Integer(128);
System.out.println(i9 == i10); //true

//这个是new出来的，new出来的都是不同的对象 == 判断的就是两个对象是否相等，所以是fasle
Integer i1 = new Integer(1);
Integer i2 = new Integer(1);
System.out.println(i1 == i2); //false  

//这个底层用的是Integer.valueOf() 使用缓存的条件是-128～127 
Integer i5 = 127; 
Integer i6 = 127;
System.out.println(i5 == i6); //true

//一个使用的是Integer.valueOf() 一个是new出来的 肯定是两个不同的对象
Integer i7 = 127; 
Integer i8 = new Integer(127);
System.out.println(i7 == i8); //fasle


```

