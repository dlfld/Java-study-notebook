# Java泛型

 ```java
 class Person<E>{
   E s;//E表示s的数据类型，该数据类型在定义Person对象的时候指定，即在编译期间，就确定E是什么类型
   public person(E s){
     this.s = s;
   }
   public E f(){
     return s;
   }
 }
 
 Person<String> person = new Person<String>("aaa");//这样就表示E变为String了
 ```

## 泛型的语法

+ 泛型的声明
  + interface接口<T>{}和class类<K,V>{},其中TKV不代表值，而是代表类型
  + 泛型指定类型一般是在创建类的时候
  + T E泛型在指定的时候只能够是引用类型，不能够是基本数据类型
  + 在给泛型指定了具体类型后，可以传入该类型和该子类类型



## 自定义泛型

基本语法 interface/class name <T,R….>   …表示可以有多个泛型

+ 普通成员可以使用泛型
+ 使用泛型的数组，不能初始化。 因为数组在new 不能确定T的类型就无法在内存开空间
+ 静态方法中的不能使用类的泛型。 因为静态方法和对象没关系，是属于这个类的，有可能在使用静态方法的时候，类没有初始化为对象，因此就不知道这个泛型具体是什么数据类型的。
+ 泛型类的类型，是在创建对象时确定的（因为创建对象时，需要指定确定类型）。如果是接口泛型的话就是继承和实现接口的时候指定类型 的。
+ 如果在创建对象时，没有指定类型，默认为Object

```java
public class Custom<U,M> {
    public void run() {
    }//普通方法


    /**
     * 泛型方法
     * 是提供给fly使用的 
     * 泛型方法 可以使用类里定义的泛型，也能够使用自己定义的泛型
     * <T, R> 这儿是定义泛型
     * @param t
     * @param r
     * @param <T>
     * @param <R>
     */
    public <T, R> void fly(T t, R r) {
    }
  
  /**
  *	不是泛型方法，这个这是方法中使用了泛型
  *
  */
  	public void hi(U u){}
}
```

## 泛型的继承和通配符

+ 泛型不具备继承性。
+ <?> 支持任意泛型类型
+ <? extends A> 支持A类以及A类的子类，规定了泛型的上限
+ <? super A> 支持A类及A类的父类，不限于直接父类，规定了泛型的下限

```java
public class Custom {
    // 说明： list<?> 表示任意的泛型类型都可以接收
    public static void printCollection1(List<?> c) {
        for (Object object : c) { //通配符，取出时，就是Object
            System.out.println(object);
        }
    }

    //？extends AA 表示上限，可以接收AA或者AA子类
    public static void printCollection2(List<? extends AA> c) {
        for (Object object : c) { //? extends AA 表示上限 可以接受AA或者AA子类
            System.out.println(object);
        }
    }
    //？ super 子类类名AA 支持AA类以及AA类的父类，不限于直接父类
    public static void printCollection3(List<? super AA> c) {
        for (Object object : c) { //? super子类类名AA 支持AA类以及AA类的父类，不限于直接父类，规定了泛型的下限
            System.out.println(object);
        }
    }
}


class AA {

}

class BB extends AA {

}

class CC extends BB {

}
```