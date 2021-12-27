# **状态模式（State）**

状态（State）模式：允许一个对象在其内部状态发生改变时改变其行为能力。比如QQ来说，有几种状态，在线、隐身、忙碌等，每个状态对应不同的操作，而且你的好友也能看到你的状态，所以，状态模式就两点：1、可以通过改变状态来获得不同的行为。2、你的好友能同时看到你的变化。看图：

![img](状态模式（State）.assets/20190315154046872.png)

State类是个状态类，Context类可以实现切换，我们来看看代码：

```java
/**
 * 状态类的核心类
 */
public class State {
	
	private String value;
	
	public String getValue() {
		return value;
	}
 
	public void setValue(String value) {
		this.value = value;
	}
 
	public void method1(){
		System.out.println("execute the first opt!");
	}
	
	public void method2(){
		System.out.println("execute the second opt!");
	}
}
```

```java
/**
 * 状态模式的切换类
 */
public class Context {
 
	private State state;
 
	public Context(State state) {
		this.state = state;
	}
 
	public State getState() {
		return state;
	}
 
	public void setState(State state) {
		this.state = state;
	}
 
	public void method() {
		if (state.getValue().equals("state1")) {
			state.method1();
		} else if (state.getValue().equals("state2")) {
			state.method2();
		}
	}
}
```

 测试类：

```java
public class Test {
 
	public static void main(String[] args) {
		
		State state = new State();
		Context context = new Context(state);
		
		//设置第一种状态
		state.setValue("state1");
		context.method();
		
		//设置第二种状态
		state.setValue("state2");
		context.method();
	}
}
```

输出：

execute the first opt!
execute the second opt!

根据这个特性，状态模式在日常开发中用的挺多的，尤其是做网站的时候，我们有时希望根据对象的某一属性，区别开他们的一些功能，比如说简单的权限控制等。