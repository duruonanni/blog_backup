---
title: Java基础重修_01关键字this&super
date: 2018-01-03 17:45:29
update: 2018年1月4日 12点02分
tags: 
    - 笔记
    - Java
    - 基础知识
categories: 
    - 笔记
    - Java
---

## 写在之前的话
- 踩着2017的尾巴,重新启用此博客
- 之前的Java笔记,原本想把Java基础的知识尽量多写一遍,这个估计要承[富坚义博][3]之志啦
- 这个系列主要是记录一些Java基础的难点,参杂一些Java8的新特性
	- 主要是杜若汇集[<Java 核心技术 卷一>][2]中遇到的难点和问题
	- 本系列适合掌握了Java基础知识的同学进行重点和难点的复习 
呐~正文开始啦~🐱‍👤 

## this关键字的作用
- 概述
	1. `this`关键字,可以直接调用本类的成员变量和成员方法
		- `this.xxx`
	2. 充当当前类对象的引用
		- `return this`
	3. 在构造方法中,可以用来调用其他构造方法
		- `this(paras...)` 

### 作用1 : 调用类中的成员变量&成员方法
- 在创建一个`JavaBean`时,通常会为其提供get和set方法,用于防止直接访问到类中的成员变量.其中,就用到了this关键字,eg: 
	- 在此示例中,主要解决了成员变量和形式参数的重名问题;
	- 语法是: `this.成员变量名` 和 `this.方法名`
```
public class Student {
	// 1.本类成员变量的age
	private int age;
	public int getAge() {
		return age;
	}
	public void setAge(int age) { // 2.形参中的变量age
		// 观察可知 : 方法中形参名为age,本类中也有个成员变量名为age.这里就出现了变量的同名
		// 在Java中对重名变量的处理遵循就近优先的原则("同名变量的屏蔽原则"),即方法内的优先于方法外(成员变量)的.这里就是默认全部识别为形参的age(成员变量默认被隐藏)
		// 想要在方法中调用成员变量的age,就可以使用this关键字
		// 这里的this指的是当前类,this.age就是当前类下的成员方法
		// 这个方法的意思就是 : 将形参接收到的age值(2),传递到类中成员变量位置上的age(1)之中
		this.age = age;
	}
	
	private String name;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	// this关键字调用本类方法的简单示例
	public void sayHello() {
		System.out.println("Hello,My name is" + this.getName() + " I'm " + this.getAge() + " year old.");
	}
}
```

### 作用2 : 充当当前类对象的引用
- 这是一种特殊的用法,使用`this`指代当前对象本身
	- 诸如,允许这样使用`Student student = this`
	- 可以把`this`当某个方法的返回值返回
		- 注意,这个方法必须是非静态方法
		- 因为需要通过new创建了对象,使用`this`才有意义
	- 这种方法只有`this`能用,`super`中没有对应的这种用法
```
// JavaBean类
public class Student02 {

	private String name;
	private int age;
	public Student02() {}
	// 有参构造方法传递值,进行初始化参数
	public Student02(String name, int age) {
		this.name = name;
		this.age = age;
	}
	// 方法内初始化参数,使用this进行返回
	public Student02 getStudent02() {
		name="lumia";
		age=6;
		return this;
	}
	@Override
	public String toString() {
		return "Student02 [name=" + name + ", age=" + age + "]";
	}

}
// 测试方法
public static void main(String[] args) {
	Student02 student1 = new Student02("Samsung", 36);
	Student02 student2 = new Student02().getStudent02();
	// 返回结果: 使用构造方法生成:Student02 [name=Samsung, age=36]
	System.out.println("使用构造方法生成:" + student1);
	// 返回结果: 使用this返回值生成:Student02 [name=lumia, age=6]
	System.out.println("使用this返回值生成:" + student2);
}
```

### 作用3 : 构造方法中的作用
- 前言: 这种用法似乎并不太常用,大概知道下就好吧
- 一个`JavaBean`可以有多个构造方法,此时,可通过`this`关键字简化构造方法的创建;
- 在多参数的构造方法中,可以直接调用参数少的构造方法获取的值,示例如下:
- 语法: 
	- 创建好一个少参数构造方法后,在创建更多参数的构造方法时,在新构造方法首句使用
	- `this(形参1,形参2)`
	- 这样,新构造方法中,`形参1`和`形参2`的值就会调用那个少构造参数的方法赋值
- 注意事项:
	- 这种方法只有放在新构造方法的首句才有效,否则会报错;
	- 系统会自动根据参数类型识别`this`指代的构造方法;
	- 实际上,每个构造方法中都能这样使用`this`,无论传入参数的多少(以上少到多只是为了方便举例)
```
public class ConstructorThisTest {

	public static void main(String[] args) {
		Test test = new Test("迷麟", "迷麟", 100, 100);
		System.out.println(test);
	}
	
}

class Test {
	
	private String name1;
	private String name2;
	private int health1;
	private int health2;
	
	public Test() { } // 无参构造方法

	public Test(String name2, int health2) { // 简单构造方法1
		this.name2 = "this+" + name2;
		this.health2 = health2 + 100;
	}

	// 复杂构造方法2
	public Test(String name1, String name2, int health1, int health2) {
		// 使用this调用简单构造方法1取值,注意必须放在方法首句
		this(name2,health2);
		this.name1 = name1;
		this.health1 = health1;
	}

	@Override
	public String toString() {
		return "test [name1=" + name1 + ", name2=" + name2 + ", health1=" + health1 + ", health2=" + health2 + "]";
	}
	
}
```

## super关键字的作用
- 概述:
	- super关键字主要用于具备继承关系的类中,从子类获取父类的信息;
	- 其主要用法类似于this,以下稍做介绍
	- 作用1 : 同this作用1**添加超链接**
		- 调用父类中的成员变量&成员方法
		- 避免类或者方法的同名屏蔽
		- 语法: `super.xxx`
	- 作用2 : 同this作用3**添加超链接**
		- 在构造方法中的相互调用
		- 语法: `super(paras...)`
- 注意事项:
	- super不具备充当类对象引用的作用(this的作用2)
	- 所有的自定义类都继承自Object类,所以所有自定义类都可以使用super;
	- 此外,super关键字在`泛型类`的`通配符`类型中还可以进行`超类的限定`
		- 具体用法见`泛型篇` **TODO**
- 用法示例:
```
// 总结:
	// 子类使用new创建对象时,先调用父类的构造方法,再调用子类对应的构造方法
	// 用法1:通过super.xxx调用到父类对应的方法,sayHello
	// 用法2:使用super(paras...)构造方法,调用父类的构造方法
public class InheritSuperTest {
	public static void main(String[] args) {
		Childd child0 = new Childd();
		System.out.println(child0);
		System.out.println("------------");
		Childd child2 = new Childd("子类实例");
		System.out.println(child2);
	}
}

class Parentt {
	private String name = "Parent";
	public String getName() {
		return name;
	}
	public Parentt() {
		System.out.println("父类空参构造");
	}
	public Parentt(String name) {
		this.name = name;
		System.out.println("父类有参构造");
	}
	public void sayHello() {
		System.out.println("父类SayHello");
	}
	
}

class Childd extends Parentt {
	private String name = "child";
	public Childd() {
		// super用法1
		super.sayHello();
		System.out.println("子类空参构造");
	}
	public Childd(String name) {
		// super用法2
		super(name);
		System.out.println("子类有参构造");
	}
	@Override
	public void sayHello() {
		System.out.println("子类SayHello");
	}
	@Override
	public String toString() {
		return "Childd [name=" + name + "]";
	}
}
```

## this在继承关系下的注意事项
- `this`在继承关系(纯子类和多态类)下,调用比较诡异复杂,容易出错;
- 以下是我的总结和代码示例,请结合代码示例来看总结;
```
//总结:
	// 1. 使用this(xxx)调用构造方法,在多态类和纯子类下,都只会调用到父类对应的方法(如果父类没有这样的构造方法,编译无法通过)
	// 2. 使用this.xxx调用类中成员变量,若子类重写(覆盖)了这个变量,在多态类和纯子类下,都只会调用到子类的成员变量(如果没有重写,则调用父类的)
	// 2.1 对于多态类来说,使用 对象名.成员变量 ,是调用父类的成员变量(如果父类没有这个成员变量,编译无法通过)
	// 2.2 对于纯子类来说,使用 对象名.成员变量,是调用子类的成员变量(如果子类没有,则调用父类的)
	// 3. 使用this.yyy(paras...)调用类中的成员方法,若子类重写了该方法,在多态类和纯子类下,都只会调用子类的方法(没有重写就调用父类的)

public class InheritThisTest {
	public static void main(String[] args) {
		Parent test = new Child("world"); // 多态类实例
		// 调用方法参见,总结: 1,2,2.1
		System.out.println("多态类 使用this引用: " + test.helloTest() + "; 直接引用: " + test.hello);
		Child test2 = new Child("world"); // 纯子类实例
		// 调用方法参见,总结: 1,2,2.2
		System.out.println("纯子类 使用this引用: " + test2.helloTest() + "; 直接引用: " + test2.hello);
	}
}

class Parent {
	public String hello = "Hello Parent ";
	Parent() {
		System.out.println("父类空参构造方法");
	}
	Parent(String hello) {
		this();
		System.out.println("父类有参构造方法: " + this.hello);
	}
	public int changeNum(int num) {
		return num+1;
	}
	public String helloTest() {
		System.out.println(this.changeNum(10));
		return this.hello;
	}
}


class Child extends Parent {
	// 可通过注释下面变量验证,成员变量未被子类覆盖时的情况
	public String hello = "Hello Child ";
	public Child() {}
	public Child(String hello) {
		// 1. 调用空参构造,观察使用父类还是子类的方法
		this();
		System.out.println("子类有参构造方法: " + this.hello);
	}
	// 可通过注释下面方法验证,成员方法未被覆盖时的情况
	@Override
	public int changeNum(int num) {
		return num-1;
	}
	@Override
	public String helloTest() {
		System.out.println(this.changeNum(10));
		return this.hello;
	}
}
```

至此,完...... 
<div align="center">![20th boy][4]</div>

<!-- 参考文献 -->
[1]: https://code.visualstudio.com/ "vscode"
[2]: http://product.dangdang.com/24035306.html "Java 核心技术 卷一"
[3]: https://zh.moegirl.org/%E5%AF%8C%E3%AD%B4%E4%B9%89%E5%8D%9A "富坚义博"
[4]: http://storage.live.com/items/AEE68C12565C1619!199122?authkey=AJoh90nl3u6Wj4U "Album Cover 20th boy"