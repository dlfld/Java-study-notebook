# HashCode

散列码（hashcode）是由一个对象导出的一个整型值。散列码是没有规律的，如果x和y是两个不同的对象那么他们的hashcode基本上不会相同。如果两个对象相同则他们的hashcode一定相同，但是如果两个对象的hashcode相同，两个对象不一定拥有相同的hashcode。

String 类使用以下算法计算散列码

![image-20220218134213390](hashCode.assets/image-20220218134213390-5162934.png)

由于 hashcode方法定义在0 bject类中，因此每个对象都有一个默认的散列码，其值由对象的存储地址得出。

字符串的散列码是由内容导出的，对象的散列码是根据对象的存储地址得到的。如果重新定义了equals方法，就必须为用户可能插入散列表的对象重新定义hashcode方法。

详解 https://www.cnblogs.com/Qian123/p/5703507.html