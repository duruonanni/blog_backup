---
title: Java基础重修_04Java中的泛型
date: 2018-01-15 21:45:39
tags:
    - 笔记
    - Java
    - 基础知识
    - 泛型
categories:
    - 笔记
    - Java
---

## 写在之前的话
初学泛型时,只知道它广泛应用在集合中,可以对集合中存入的数据类型进行限定.  
对于带通配符的泛型类,泛型方法,泛型接口,也常会遇到,但并不太明白具体概念和使用.  
正好,`<Java 核心技术>`使用了一章的内容来专门介绍泛型. 
就利用此书重学下泛型,并将学习记录整理于此文. 
~~主要是填之前,[this&super关键字][1]一文的坑~~ 
呐~正文开始啦~🐱‍👤

### 泛型简介
- 泛型(Genericity),是一种编程语言特性,它提供了`类型参数化`的能力
- 下面通过一段简单的代码来解释泛型的作用: 

```
public class ListGenericity {

	public static void main(String[] args) throws Exception{
		ListWithoutGenericity();
		ListWithGenericity();
	}

	public static void ListWithoutGenericity() {
		List strList = new ArrayList();
		strList.add("One");
		strList.add("Two");
		strList.add(3);
		// 执行以下时抛出类型转换异常
		String str2 = (String) strList.get(2);
		System.out.println(strList);
	}
	
	public static void ListWithGenericity() throws Exception{
		List<String> strList = new ArrayList<>();
		strList.add("One");
		strList.add("Two");
		// strList.add(3); // 由于泛型的限定,无法通过编译
		// 因为Java的泛型在运行时执行类型擦除,所以可以通过反射添加不符合泛型的数据
		strList.getClass().getMethod("add", Object.class).invoke(strList, 3);
		System.out.println(strList);
	}
}
```
#### 以上代码说明:
1. 在Java中`ArrayList`可以存放任意引用类型数据,因为其内部元素是Objcet,在运行时再向下转型,这样的缺点是,容易造成类型不安全
    - 比如: 存入了错误的数据类型,在取用时由于类型错误导致异常
2. 泛型在此处对`ArrayList`中的数据进行了详细的限定,通过`List<String>`,声明存入的数据类型必须是`String`及其子类,从而避免类型不安全出现的错误
    - 这里的String类型是详细指明了的,此处不是`类型参数`用法
    - ~~关于`类型参数`后面会再讲~~
		- (坑了,后面也没有讲泛型擦除,不知道需要说什么...残念)
3. 在Java中,泛型是一个编译时的检查过程,此处实际代码运行时仍然是Object类型,因此可以通过动态反射越过泛型实现其他类型数据的添加 

#### 泛型的好处
1. 减少类的创建数目,促进代码复用性
2. 避免类型转换造成的运行时错误
3. 泛型的出现,为创建容器类提供了方便

## 类型参数的泛型用法
使用`类型参数`的泛型,就是将泛型中的具体类型,用一个虚拟参数代替. 
在对应的泛型数据实例化时,再具体指定泛型的类型.
示例如下:
- `ArrayList<Integer>` : 这是一个`参数化`的泛型,`Integer`称为实际类型参数
- `ArrayList<E>` : 这是一个带`类型参数`的泛型,其中`E`称为`类型参数` 
- `ArrayList` : 是泛型类的原始类型 

### 常用类型参数标记符
1. T - Type : 常用于Java类
	1.1 多种参数类型时常用T后面的字母表示如: U , V 等
2. E - Element : 常用于集合
3. K,V - Key,Value : 常用于Map中的键值对
4. ? - 表示不确定的类型

### 简单泛型类
以下是一个简单的泛型类:
```
public class Pair<T, U> {
	
	private T first;
	private U second;
	
	public Pair() {	}
	public Pair(T first, U second) {
		this.first = first;
		this.second = second;
	}
	public T getFirst() {
		return first;
	}
	public void setFirst(T first) {
		this.first = first;
	}
	public U getSecond() {
		return second;
	}
	public void setSecond(U second) {
		this.second = second;
	}
	
}
```
以上泛型类的"实例化"以及测试方法:
```
public class PairTest {

	public static void main(String[] args) {
		String[] strs = {"abc","123","qwert","aaa"};
		Pair<String,String> test = minmax(strs);
		System.out.println(test.getFirst()+ "..." + test.getSecond());
	}
	
	public static Pair<String,String> minmax(String[] strs) {
		if (strs == null || strs.length == 0) 
			return null;
		String min = strs[0];
		String max = strs[0];
		for (int i = 1; i < strs.length; i++) {
			min = strs[i].compareTo(min)>0 ? min : strs[i];
			max = strs[i].compareTo(max)<0 ? max : strs[i];
		}
		return new Pair<String,String>(min,max);
	}
}
```
#### 简单泛型类的说明
1. 泛型类在声明时,使用单个大写字母指代类型
    - 这里指代的类型,是给类中变量和方法使用的
2. 在创建类的实例时,需要将`类型参数`具体化
    - eg: `pair<Integer,String>`
    - 参数类型必须是引用类型,不能使用基本数据类型
3. 向泛型对象存入基本数据类型值,会发生自动拆装箱
    - 应该尽量避免这样操作,因为自动拆装箱会消耗性能
4. Java不支持泛型化数组
5. 泛型类中无法定义静态泛型成员
    - 因为静态成员实例化时先执行,必须明确其类型
6. <span id="anchor1">泛型类型不能直接或间接的继承自`Throwable`</span>
	- 因为类型不确定
	- 这意味着不能抛出或捕获泛型类的异常对象
7. 泛型类派生子类:
	- 子类需要指明数据类型,或者使用默认的object类型 
8. 泛型类型的继承关系:
	- 泛型类是其原始类型的子类,和泛型实例化后其内部子父类关系无关
	- 可以联想`ArrayList<Manager>`和`ArrayList<Employee>`并无子父类关系


### 泛型方法

#### 泛型方法概述及说明
1. 泛型方法是使用了`类型参数`泛型的方法
	- 泛型参数可以是方法的返回值,或者形参
2. 泛型方法可以再泛型类或者普通类中定义
3. 在调用该方法时,编译器能自动通过传入实际参数或者用于接收的返回值推断`类型参数`的具体化值
	- 所以可以像普通方法那样不在调用时说明参数类型 

#### 泛型方法代码示例:
```
public class Lists {
	
	// 泛型方法中,使用"..."可变参数很方便
	static <T> List<T> toList(T... arr) {
		List<T> list = new ArrayList<T>();
		for(T a: arr) 
			list.add(a);
		return list;
	}
	
	public static void main(String[] args) {
		// 注意实例化时,并未注明泛型方法的具体类型,编译器可以"自动推断"
		List<Integer> ints = Lists.toList(1,2,3);
		List<String> strs = Lists.toList("hello","genericity");
		System.out.println("ints:" + ints.toString() + " ; strs:" + strs.toString());
	}
}
```

### 泛型接口

#### 泛型接口概述及说明
1. Java中的接口也可以定义泛型
2. 泛型接口的最广泛示例是`生成器`
	2.1 使用工厂模式时,其内部通常需要提供一个方法生产对象
	2.2 生成器方法的返回值通常是该类的实例
	2.3 使用泛型接口做生成器的好处 : 被实现时才确定类型,适用性更广
		2.3.1 不使用泛型,就只用Object类型,那样实现时强转不安全 

#### 泛型接口示例
- 示例说明:
	- 采用两个类的对比说明泛型接口的优势
	- 在示例中,顺便实现了[泛型类型不能直接或间接的继承自`Throwable`](#anchor1)的示例
	- 注意观察示例中的注释,便于理解代码 

1. 示例容器类(Juice和Monkey) 

```
public class Juice{
	
	@Override
	public String toString() {
		return this.getClass().getSimpleName();
	}
	
}
class Apple extends Juice{}
class Orange extends Juice{}
class Peach extends Juice{}
class Banana extends Juice{}

// 非果汁类
class Monkey{
	@Override
	public String toString() {
		return this.getClass().getSimpleName();
	}
}
```
2. 示例1 : 带泛型接口的方法 

```
// 带泛型接口
interface GenericGenerator<T> {
	T next();
}

public class JuiceGenerator implements GenericGenerator<Juice> {

	private Class[] types = {Apple.class,Orange.class,Peach.class,Banana.class};
	private Random random = new Random();

	@Override
	public Juice next() { // 实现方法时,必须实例化返回值类型
	// 说明: 此处为什么使用try-catch
		// 因为方法继承自泛型接口,而泛型接口不能继承Throwable接口,所以只能在方法内处理异常
		try {
			// return Monkey.class.newInstance(); // 因为Monkey不是Juice的子类,无法通过编译,避免错误
			return (Juice) types[random.nextInt(types.length)].newInstance();
		} catch (InstantiationException | IllegalAccessException e) {
			// 说明: 在catch中 throw RuntimeException的原因
				// 因为要保证方法绝对有返回值
				// try语句中有return,但如果抛异常来到catch语句中,就必须通过抛出运行时异常使程序运行停止,来避免缺少此处的返回情况
			throw new RuntimeException(e);
		}
	}
	
	public static void main(String[] args) {
		JuiceGenerator gen = new JuiceGenerator();
        for (int i = 0; i < 5; i++)
            System.out.println(gen.next());
	}

}
```
3. 示例3: 不带泛型接口的方法 

```
// 不带泛型的接口,返回值使用Object
interface UnGenericGeneritor {
	Object next() throws Exception;
}

public class JuiceGenerator2 implements UnGenericGeneritor{

	private Class[] types = {Apple.class,Orange.class,Peach.class,Banana.class};
	private Random random = new Random();
	
	@Override
	public Object next() throws Exception { // 由于接口不带泛型,所以方法可以抛异常
		
		// return types[random.nextInt(types.length)].newInstance();
		return Monkey.class.newInstance(); // 由于返回值类型是Object,所以Monkey可以被返回,这就造成了不安全
		
	}
	
	public static void main(String[] args) throws Exception {
		JuiceGenerator2 gen = new JuiceGenerator2();
        for (int i = 0; i < 5; i++)
            System.out.println(gen.next());
	}

}
```

### 泛型约束

#### 泛型约束概述及说明
1. 使用泛型约束可以在泛型类`声明时`对泛型参数进行限制
	- 即要求满足某接口或子类等条件才能实例化
	- 约束关键词是`extends`
2. 约束示例: 
	- `ClassName(T extends BoundingType)`
	- 其中T和BoundingType可以是类也可以是接口
	- T必须满足是BoundingType的子类或子接口
2. 注意: 
	- 可以添加多个约束,使用`&`为分隔符号;
	- 多个约束条件时最多只能有一个是类,而且类必须写在约束的最前面
	- 泛型约束中没有`super`关键字,要和通配符类型的泛型进行区分 

#### 泛型约束代码示例: 

```
public class GenericityConstriantTest {

	TestClass<Integer> num1 = new TestClass<>();
	TestClass<Double> num2 = new TestClass<>();
	TestClass<Float> num3 = new TestClass<>();
	// String不是数字类型,所以下面的实例化会在编译时报错
	TestClass<String> str = new TestClass<>();
	
}

// Number 是Integer和Double的抽象超类
	// 此约束说明实例化该类时,类型必须是Number的子类,也就是数字类型
class TestClass<T extends Number> {
	T num;

	public T getNum() {
		return num;
	}
	public void setNum(T num) {
		this.num = num;
	}
	
}
```
### 类型通配符 : ?

#### 类型通配符概述: 
这是泛型的最后一个难点了,也是最难的.这里来慢慢整理一下.  
之前的学习中,我们的泛型类实例化时,都需要将参数类型具体化,这样的缺点是在参数具备子父类关系时,易受限制. 
示例如下: 
```
public static void printBuddies(Pair<Employee> p) {
	Employee first = p.getFirst();
	Employee second = p.getSecond();
	System.out.println(first.getName() + " and " + second.getName() + " are buddies");
}
```
示例中的方法是,打印两个员工的姓名. 
因为`Pair<Employee> p`的限制,所以调用该方法时,就无法传递`Employee`的子类`Manager`的实例来调用此方法. 
所以,引入了通配符类型的概念:
`Pair<? extends Employee>`
他是`Pair<Emloyee>`和`Pair<Manager>`的子类.
这就表示,传递的参数只要符合是`Employee`的子类,就能顺利调用方法.
#### 类型通配符注意事项:
1. 通配符不是变量,不能在代码中使用`?`作为一种类型
	- 这意味着,不能出现下面的代码:
	- `? a = getFirst();`
	- 由于类型不确定,也无法使用`set`方法执行赋值
	- 但是,可以使用get方法判断返回值是否为`null`:
	- `p.getFirst() = null`

#### 类型通配符说明:

##### 类型通配符分类:
1. 子类限定
	- `ClassName<? extends BoundingType>`
	- 关键词: `extends`
	- 即,要求传入的实参必须是BoundingType及其子类
2. 超类限定
	- `ClassName<? super BoundingType>`
	- 关键词: `super`
	- 即,要求传入的实参必须是BoundingType及其子类
3. 无限定通配符
	- `ClassName<?>`
	- 对传入的实参类型不做要求
	- 这个需要和参数为Object的非泛型类进行区别
	- 即,无限定通配符类是所有该容器类的子类
		- eg1: `Collection<Object>`和`Collection<Integer>`无子父类关系,不使用泛型,则无法利用多态相互转换
		- eg2: `Collection<?>`是 `Collection<Object>`和`Collection<Integer>`的父类,所以使用此泛型,容器类中存入任意类型都能顺利使用多态 

##### 通配符捕获
- 概念描述:
	- 之前有提到通配符类型泛型类,由于`?`不属于一种类型,所以无法对方法中的数据进行进一步操作(不知道类型就无法操作参数)
	- 对于这样的情况,应该避免使用带通配符的泛型
	- 如果无法避免使用,则可以通过构造一个带泛型的辅助方法来执行操作
	- 具体示例见: [PairAlg类](#anchor2) 

#### <Java 核心技术>类型泛型示例解析
- 下面是书中的类型泛型示例代码
- 我删减了代码中的一些多余部分,并添加注释
- 建议复制代码尝试运行与观察
- 具体代码如下: 

##### Employee类
- Manager类的父类
- 提供name和salary两个参数,和参数的getter,setter方法 

```
public class Employee {

	private String name;
	private double salary;
	
	public Employee(String name, double salary) {
		this.name = name;
		this.salary = salary;
	}
	public Employee() {}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	
}
```
##### Manager类
- Manager类继承自Employee类
	- 继承了name和salary两个参数
- 自有一个bonus参数
- 重写了getSalary()方法,目的是将salary基本工资和bonus红利相加 

```
public class Manager extends Employee {

	private double bonus;

	public Manager(String name, double salary) {
		super(name, salary);
		bonus = 0;
	}

	public double getBonus() {
		return bonus;
	}

	public void setBonus(double bonus) {
		this.bonus = bonus;
	}
	
	@Override
	public double getSalary() {
		double baseSalry = super.getSalary();
		return baseSalry + bonus;
	}
	
}

```
##### 泛型类Pair<T>
- 可以存入两个值
- 并为两个值提供了getter和setter方法 

```
public class Pair<T> {

	private T first;
	private T second;
	
	public Pair() {}
	public Pair(T first, T second) {
		this.first = first;
		this.second = second;
	}
	
	public T getFirst() {
		return first;
	}
	public void setFirst(T first) {
		this.first = first;
	}
	public T getSecond() {
		return second;
	}
	public void setSecond(T second) {
		this.second = second;
	}
	
}
```
##### PairAlg类
- 用于判定Pair类中存入的两个值是否为空的工具类
- Pair类的实现使用了通配符类型泛型
- 此处主要为了展现<span id="anchor2">通配符捕获</span>的用法 

```
// 作用是交换Pair<?> 类中两个参数的值
class PairAlg {
	// 使用无限定通配符参数判定空值
	public static boolean hasNulls(Pair<?> p) {
		return p.getFirst() == null || p.getSecond() == null;
	}

	// 调用下面的辅助方法,间接达到操作通配符泛型方法的目的
	public static void swap(Pair<?> p) {
		swapHelper(p);
	}

	// 引入swapHelper泛型方法实现了: 通配符捕获
	public static <T> void swapHelper(Pair<T> p) {
		T t = p.getFirst();
		p.setFirst(p.getSecond());
		p.setSecond(t);
	}

}
```

##### PairTest类
- 综合介绍通配符类型泛型用法的类
- 添加了注释帮助理解代码 

```
public class PairTest {

	public static void main(String[] args) {
		Manager ceo = new Manager("Ging", 800000);
		Manager cfo = new Manager("Killua", 600000);
		Manager cao = new Manager("Gon", 500000);

		Pair<Manager> buddies = new Pair<>(cfo, cao);
		printBuddies(buddies);

		ceo.setBonus(200000);
		cfo.setBonus(500000);
		cao.setBonus(600000);
		Manager[] managers = { ceo, cfo, cao };
		Pair<Employee> result = new Pair<>();
		minmaxBonus(managers, result);
		System.out.println("first: " + result.getFirst().getName() + ", second: " + result.getSecond().getName());
		maxminBonus(managers, result);
		System.out.println("first: " + result.getFirst().getName() + ", second: " + result.getSecond().getName());
	}

	// 方法作用是: 打印传入的两位经理name信息
	public static void printBuddies(Pair<? extends Employee> p) { // 使用通配符泛型子类限定
		// Pair<? extends Employee> 是 Pair<Manager>的父类,所以可以正确执行
		Employee first = p.getFirst();
		Employee second = p.getSecond();
		System.out.println(first.getName() + " and " + second.getName() + " are buddies");
	}

	// 方法作用是: 找出数组中经理红利Bonus最多和最少的人
	public static void minmaxBonus(Manager[] a, Pair<? super Manager> result) {
		// Pair<? super Manager> 使用通配符泛型超类限定
		if (a.length == 0)
			return;
		Manager min = a[0];
		Manager max = a[0];
		for (int i = 0; i < a.length; i++) {
			if (min.getBonus() > a[i].getBonus())
				min = a[i];
			if (max.getBonus() < a[i].getBonus())
				max = a[i];
		}
		result.setFirst(min);
		result.setSecond(max);
	}

	// 方法作用是,对minmaxBonus方法的值进行交换
	// 调用了工具类PairAlg的方法
	public static void maxminBonus(Manager[] a, Pair<? super Manager> result) {
		minmaxBonus(a, result);
		PairAlg.swapHelper(result);
	}

}
```

至此,完......
<div align="center">![Album_Cover_异国迷路のクロワーゼ][2]</div>


<!-- 参考文献 --> 
[1]: http://duruonanni.com/Renovate-Java-Basic-01keyworld-this/20180103.html "Renovate-Java-Basic-01keyworld-this"
[2]: http://storage.live.com/items/AEE68C12565C1619!201551?authkey=AJoh90nl3u6Wj4U "Album_Cover_异国迷路のクロワーゼ"