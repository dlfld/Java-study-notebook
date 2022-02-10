# Java反射

+ 反射机制允许程序在执行期间借助于Reflection 的Api 取得任何类的内部信息（比如成员变量，构造器，成员方法等等），并能操作对象的属性及方法。反射在设计模式和框架底层都会用到。

+ 加载完类之后，在堆中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象包含了类的完整结构信息。通过这个对象得到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，形象的称之为：反射。

  ![image-20211115215505555](图片文件存储/image-20211115215505555.png)

## Java反射机制可以完成

+ 在Java运行时判断任意一个对象所属的类
+ 在运行时构造任意一个对象
+ 在运行时得到任意一个类所具有的成员变量和方法
+ 在运行时调用任意一个对象的成员变量和方法
+ 生成动态代理

## 反射的优点和缺点

+ 优点：可以动态的创建和使用对象（也是框架底层核心），使用灵活，没有反射机制，框架技术就失去底层支撑
+ 缺点：使用反射机制基本就是解释执行，对执行速度有影响

## 反射调用优化-关闭访问检查

访问检查：语言访问控制检查的能力。对于公共成员、默认(打包)访问成员、受保护成员和私有成员，在分别使用Field、Method和 Constructor对象来设置或获得字段、调用方法，或者创建和初始化类的新实例的时候，会执行访问检查。

+ Method和Field\Constructor对象都有setAccessible()方法。
+ setAccessible作用是启动和禁用访问安全检查的开关
+ 参数为true表示反射的对象在使用时取消访问检查，提高反射效率。参数值为false则表示反射的对象执行访问检查

## Class类

+ Class也是类，因此也继承Object类

+ Class类对象不是new出来的，而是系统创建出来的

+ 对于某个类的Class类对象，在内存只有一份，因为类只加载一次

+ 每个类的实例都会记得自己是由那个Class实例所生成

+ 通过Class可以完整地得到一个类的完整结构，通过一系列API

+ Class对象是存放在堆的

+ 类的字节码二进制数据，是放在方法区的，有的地方称为类的元数据

+ 

+ ```java
  /**
   *  直接new的方式
   *     public Class<?> loadClass(String name) throws ClassNotFoundException {
   *         return loadClass(name, false);
   *     }
   */
  Cat cat = new Cat();
          /**
           * 反射方式 也是通过classLoader来加载类的
           *  public Class<?> loadClass(String name) throws ClassNotFoundException {
           *                 return loadClass(name, false);
           *               }
           */
   Class aClass = Class.forName("org.cuit.Cat");
  ```

```java
Class aClass = Class.forName("org.cuit.Cat");
//通过反射获取实例
Cat cat1 = (Cat) aClass.getConstructor().newInstance();
cat1.setName("aaa");
//通过反射获取属性,如果这个地方是私有属性的话就会出问题。报错
Field name = aClass.getField("name");
//获取某一个对象的属性值
System.out.println(name.get(cat1));
//通过反射给属性赋值
name.set(cat1,"张三");
//遍历所有的属性
Field[] fields = aClass.getFields();
for (Field field: fields){
    //输出所有属性字段的名字
    System.out.println(field.getName());
}
```

## 获取Class对象的各种方式

+ 前提：已知一个类的全类名，且该类在类路径下，可通过Class类的静态方法forName()获取，可能抛出ClassNotFoundException，实例：Class cls1 = Class.forName(“java.lang.Cat”)
  + 应用场景：多用于配置文件，读取类全路径，加载类
+ 前提：若已知具体的类，通过类的class获取，该方式最为安全可靠，程序性能最高实例：Class cls2 = Cat.class
  + 应用场景：多用于参数传递，比如通过反射得到对应构造器对象。
+ 前提：已经知道某个类的实例，调用该实例的getClass方法获取Class对象，实例：Class clazz = 对象.getClass() //运行类型
  + 应用场景：通过创建好的对象，获取Class对象
+ 其他方式
  + ClassLoader cl = 对象.getClass().getClassLoader()
  + Class clazz4 = cl.loaderClass(“类的全类名”)

## 那些类型有Class对象

1. 外部类，成员内部类，静态内部类，局部内部类，匿名内部类

2. interface接口
3. 数组
4. enum枚举
5. annotation 注解
6. 基本数据类型
7. void

## 类加载

1. 基本说明

   反射机制是Java实现动态语言的关键，也就是通过反射实现类的动态加载

   1. 静态加载：编译时加载相关的类，如果没有就报错，依赖性太强

      就是在编译的时候就加载类，不管用没用到，只要在程序中出现了就要去加载。

   2. 动态加载： 运行时加载需要的类，如果运行时不用该类，则不报错，降低了依赖性

      在运行的时候加载这个类，如果这个类有错，但是运行的时候没有用到，在运行的时候就不会加载该类，也就不会报错。

2. 类加载的时机

   1. 当创建对象时（new）
   2. 当子类被加载时
   3. 调用类中的静态成员时
   4. 通过反射 

![image-20211126111343067](图片文件存储/image-20211126111343067.png)
