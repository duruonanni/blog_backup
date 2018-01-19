---
title: Java基础重修_05Lambda表达式
date: 2018-01-17 17:21:25
update:
tags:
    - 笔记
    - Java
    - 基础知识
    - Lambda
categories:
    - 笔记
    - Java
---

## 写在之前的话 

第一次知道Lambda表达式,是看<黑客与画家>中,Paul极力推崇的Lisp语言具备的重要特性. 
当时我还没学编程,只是当科普书籍在看. 
记得Paul曾在书中预言,Java将会在未来提供函数式编程的方法. 
在我开始学习Java时,知道Java8的新特性中提供了Lambda表达式的概念,就知道Paul的预言应验了. 
可之前一直没有学习这项新特性. 
最近,重读`<Java 核心技术>`,利用此机会重学下Lambda表达式,并将学习记录整理至此文. 
呐~正文开始啦~🐱‍👤

## Lambda表达式概述 

- Lambda表达式是Java8引入的一个重要新特性
- 通过Lambda表达式,可以直接传递代码块
    - 这引入了函数式编程的概念
    - 之前的Java版本中,都需要给代码块封装成类再创建对象调用
- Lambda表达式的引用使用接口接收,称为`函数式接口`
- Lambda表达式的出现,能让代码更简洁易懂,也提升了代码的可维护性
    - 可以使用Lambda表达式,替代单一方法的匿名内部类
    - 可以使用`方法引用`的表达式,让代码更简洁
- 此外,Lambda表达式还具备延迟执行的特点
    - 就是创建时不执行,调用时才执行
- 由于是全新的概念,所以其语法会稍有特殊,表达方式也多样灵活,使用时需要注意

### 简单Lambda表达式示例 

- 首先,看一个不使用Lambda表达式的例子
- 这个例子中,我们想要在main方法中,调用某个接口的方法实现
    - 首先,创建接口SayHello
    - 其次,创建接口的实现类Student
    - 再创建一个用于实现该方法的静态方法doWithSayHello
        - 实际可以省略此步骤,直接在mian中创建类的实例调用接口方法
        - 但是,对应的Lambda表达式中使用了这种中间方式
        - 为了方便对比,在此处也用上了
    - 最后在main方法中,创建类的实例,调用该方法student 

```
public class WithoutLambda {

	public static void main(String[] args) {
		// 步骤四:创建类的实例,传递到静态方法中,调用接口的方法
		Student student = new Student();
		doWithSayHello(student);
	}
	
	// 步骤三: 创建静态方法调用接口中的方法
	public static void doWithSayHello(SayHello sh) {
		sh.sayHello("Killua");
	}

}

// 步骤一:创建接口
interface SayHello {
	void sayHello(String name);
}

// 步骤二:创建接口的实现类,并在类中重写接口方法
class Student implements SayHello {
	@Override
	public void sayHello(String name) {
		System.out.println(name + " say Hello World");
	}	
}
```
- 现在,想想看,上面的代码,是不是显得有点步骤过多,我们只是想要实现一个接口的方法而已呀
- 现在直接看使用Lambda表达式后,这个问题如何解决
- 注意
    - 使用Lambda表达式,我们省略了创建接口实现类的步骤
    - 示例中,Lambda表达式2是lambda表达式1的简写(匿名)形式

```
public class WithLambda {

	public static void main(String[] args) {
		// Lambda表达式1
			// 定义了一个Lambda表达式对象sayHello2
			// =后面是接口方法的实现
			// 具体语法后面会讲
		SayHello sayHello2 = (name)->{
			System.out.println(name + "say Hello");
		};
			// 直接传递Lambda表达式对象sayHello2,到方法中
			// 省去了定义类再实现接口的工作
		doWithSayHello(sayHello2);
		
		// Lambda表达式2
			// Lambda表达式1的匿名简写方法
			// 注意各种括号和分号
		doWithSayHello((name)->{
			System.out.println(name + "sayHello");
		});
	}
	
	static void doWithSayHello(SayHello sh) {
		sh.sayHello("Killua");
	}
	
}

@FunctionalInterface
interface SayHello {
	void sayHello(String name);
}
```

## lambuda表达式语法 

- 由以上示例可知,Lambda表达式,就是直接传递一段代码
- 可以调用方法执行该代码,也可以传入某个对象中去
    - Lambda表达式对象用接口去接收
    - 这就叫函数式接口 

### 函数式接口的定义 

- 好了,这里要引入一个新概念: 函数式接口
- 函数式接口作用:
    - 可以用于接收Lambda表达式的对象
    - 方便随时调用Lambda表达式中的方法
- 函数式接口要求:
    - 内部只有一个抽象方法
- 注意事项:
    - 函数式接口,可以有: [默认方法(Default methods)](#anchor1)
    - 函数式接口,可以有: [静态方法`(Static methods)](#anchor1)
    - 函数式接口,可以有:Object类的公有方法
        - `toString`
        - `hashCode`...
    - 函数式接口,请使用注解标识清楚:
        - `@FunctionalInterface`
- 函数式接口示例: 

```
@FunctionalInterface
interface MyFunctionalInterface{
    // 函数式接口
    void func();
    
    // Object类的公有方法
    int hashCode();
    String toString();
    boolean equals(Object obj);
    
    // 接口中的默认方法
    default void defaultMethod() {
        System.out.println("Java8 新特性,默认方法是也");
    }
    
    // 接口中的静态方法
    static void staticMethod() {
        System.out.println("Java8 新特性,静态方法是也");
    }
}
```

### 补充知识点: <span id="anchor1">接口中的默认方法和静态方法</span> 

- `默认方法`(Default methods)和`静态方法`(Static methods)都是Java8的新特性
- 默认方法,特点:
    - 使用`default`关键字声明方法
    - 具备方法体
    - 只能通过其实现类调用,实现类默认拥有此方法,这意味着:
        - 实现类可以不实现这个方法,调用类时直接调用接口中的默认方法
        - 如果实现类重写了这个方法,原默认方法失效("类优先"规则)
- 静态方法,特点:
    - 使用`static`关键字定义方法
    - 具备静态方法的接口可以不通过实现类,而直接用接口名调用
    - 静态方法可以**通过接口名直接调用**,也可以在接口被类实现后,由类名调用
    - 静态方法初衷是简化某些工具类(工具类里面多是静态方法)对接口的实现,直接使用带静态方法的接口充当工具类
    - 静态方法的出现模糊了接口和类的界限,个人不太建议使用
- `默认方法`和`静态方法`相对较简单,就不单独写例子了 

### 函数式接口的实现

#### Lambda表达式1的详解
```
SayHello sayHello2 = (name)->{
    System.out.println(name + "say Hello");
};
```
- `SayHello sayHello2` 
    - 创建一个Lambda表达式对象
    - 也就是使用函数式接口的对象,接收Lambda表达式的值
- `=`
    - 将等号右边的执行结果赋值给左边
- `(...)->{ ... };`
    - 这个就是函数式接口实现的完全体
    1. `(...)` : 方法签名
        - 用于传递接口中的形式参数
        - 参数不写类型,具备类型推断的功能
            - 不使用泛型,就在定义接口方法时确定类型
            - 使用泛型的话,就在实现时确定类型咯
        - 没有参数,就写个空`()`,不可省略
        - 有参数,可以省略`()`
    2. `->` : 箭头
    3. `{ ... };` : 方法的实现(方法体)
        - 就是要执行的函数
        - 如果执行语句是一句话,可以直接写在`->`后面,不用加`{}`
        - 注意结尾分号不可少`;` 

#### Lambda表达式2的详解 

```
doWithSayHello((name)->{
    System.out.println(name + "sayHello");
});
```
- 这个就相当于一个匿名的Lambda表达式
- 不创建Lambda表达式对象,直接执行doWithSayHello方法的实现
- 语法类似匿名内部类
- 注意执行完成后的分号`;` 

### Lambda表达式注意事项: 

#### Lambda表达式访问外部变量 

- Lambda表达式方法体中,也能访问外部的变量
    - 包括其所在方法内的局部变量
    - 也包括类中`final`修饰的成员变量
    - 就和内部类访问外部信息一样
- 因为可以访问外部变量,就要注意方法签名中的变量名和外部变量的重名问题了
    - 形参名字不能和外部变量名一样,编译会无法通过 
- 变量要定义在Lambda表达式实现之前,因为代码顺序执行,放在后面调用不到 

#### Lambda表达式中使用this和super关键字 

- Lambda表达式中,使用this关键字,是指本类的引用(这个方法所在类的引用)
    - `super`关键字,就是本类的超类的引用咯
- 注意: 
    - 静态方法中无法使用`this`关键字

### 补充知识点: effectively final变量 

- 在Java8中,新添加了一个`effective final 变量`的概念,具体是说:
    - 定义变量时,不需要人为添加`final`变量,系统会默认识别
        - 所以,有些lambda表达式中调用的外部变量,没有定义为`fianl`的,也不报错
- 个人认为这个不适合新手
    - 不显示的定义出来,程序员有时候意识不到,在表达式内部对变量重新赋值,就会报错
    - 反正使用时需要牢记,不能改变外部变量的值

## Lambda表达式和匿名内部类 

- 除了自定义Lambda表达式外,Java8之后的很多接口也实现了函数式接口
    - 如,用于排序对比的`Comparator`接口
        - 它只有一个`compare`方法
- 以下示例用于对比Lambda表达式和匿名内部类 

```
public class LambdaTest1 {

	public static void main(String[] args) {
		List<String> list = getList();
		
		System.out.println("使用Lambda表达式进行排序: ");
		// 使用Lambda表达式进行排序
		lambdaSort(list);
		// 注意: 不能在main方法中直接如下使用,因为编译器无法获取方法参数的类型
			// 如果是非泛型方法,可以直接使用,不必传入方法中
		//	Collections.sort(list, (str1,str2)->str1.compareToIgnoreCase(str2););
		for (String str : list) {
			System.out.print(str + " ");
		}
		
		System.out.println();
		System.out.println("使用匿名内部类进行排序: ");
		Collections.sort(list, new Comparator<String>() {
			@Override
			public int compare(String o1, String o2) {
				return o2.compareToIgnoreCase(o1);
			}
		});
		for (String str : list) {
			System.out.print(str + " ");
		}
		
	}
	
	// 创建一个传递方法,用于实现Lambda表达式
	static void lambdaSort(List<String> strs) {
		Collections.sort(strs, (str1,str2)->str1.compareToIgnoreCase(str2));
	}
	
	// 初始化List方法
	static ArrayList<String> getList() {
		String str = "when suddenly a White Rabbit with pink eyes ran close by her";
		String[] strArray = str.split(" ");
		ArrayList<String> list = new ArrayList<>();
		for (String s : strArray) {
			list.add(s);
		}
		return list;
	}

}
```
### Lambda表达式和匿名内部类对比说明 

1. 匿名内部类需要写类名和方法名,显得更复杂
    - 如果用上后面会讲的方法引用,Lambda表达式还可以更简洁
2. Lambda表达式实现的方法如果带泛型,必须将方法的实现传递给一个中间方法
    - 示例中中间方法为: lambdaSort
    - 作用是说明泛型参数的实际类型
    - 方法不带泛型可以不使用中间方法
3. 匿名内部类相比Lambda表达式的优势是,可以实现多个方法

## 方法引用

### 方法引用概述 

- 方法引用,实际上是把方法名字直接传给一个函数型接口变量
    - 就是省略了参数,因为都知道参数传在哪个地方用
- 可以看作是Lambda表达式的简写
    - 方法引用简洁,格式优雅又方便理解
    - 有一种说法是,好的Lambda表达式,方法体都只用一句话
    - 所以,能使用方法引用时,请尽量使用方法引用
- 语法:
    - `对象名或类名::方法名`
- 方法引用可以在以下三种情况使用:
    1. `object::instanceMethod`
    2. `Class::instanceMethod`
    3. `Class::staticMethod`
- 方法引用示例1: 
    - 示例取之前代码做比较 

```
static void lambdaSort(List<String> strs) {
    Collections.sort(strs, (str1,str2)->str1.compareToIgnoreCase(str2));
    // 下面的是方法引用的写法
    //Collections.sort(strs, String::compareToIgnoreCase);
}
```
- 方法引用示例2:
    - lambda表达式方法: `x -> System.out.println(x)`
    - 方法引用: `System.out::println`
- 方法引用示例3:
    - lambda表达式方法: `x,y -> Math.pow(x,y)`
    - 方法引用: `Math::pow`
- 注意:
    - 使用方法引用,传入的值为两个以上时
    - 实现该方法时,传入值的顺序,对应方法中形参的顺序 

## 常用函数式接口

### 常用函数式接口概述: 

- Java8中,在`Java.util.function`中,帮我们定义了很多函数式接口;
    - 具体信息,参见[Java8 API][1]
- 以下列出几个较常用的函数式接口,并举例说明其用法 
    - 这些函数式接口,自己也可以写,但是API提供了,而且有些集成在了一些方法里面,用起来就很方便

### Consumer<T>接口 

#### Consumer<T>接口概述: 

- 接口名 : Consumer<T>
- 参数类型 : T
- 返回值类型 : void
- 抽象方法名 : accept
- 方法描述 : 处理一个T类型的值
- 其他方法 : andThen
- 补充知识点 : 
    - 提供了一个对应的非泛型接口: `IntConsumer`,参数为int类型时建议使用,减少执行时的拆装箱
    - 提供了处理两种类型参数的接口: `BiConsumer<T,U>`

#### Consumer<T>接口示例: 

- Java8中的iterable<T>接口,它提供了一个forEach()方法,可以接收一个Consumer<T>接口对象
    - 所以,我们可以在这个forEach()中,直接传入一个Lambda表达式,用一句话完成循环打印的要求

    <div align="center">![java_basic05_Lambda01][2]</div>
- 下面示例对比两种传统forEach和重载后的forEach
```
List<String> strs = new ArrayList<>();
strs.add("AAA");
strs.add("bbb");
strs.add("ccc");

// 传统foreach循环打印写法
for (String str : strs) {
    System.out.println(str);
}

// 使用继承自iterable接口的forEach方法,传递一个Consumer<T>函数式接口的Lambda表达式,更简单
strs.forEach(str-> System.out.println(str));
// 使用方法引用写法,更简单
// strs.forEach(System.out::println);
```

### Predicate<T>接口 

#### Predicate<T>接口概述: 

- 接口名 : Predicate<T>
- 参数类型 : T
- 返回值类型 : boolean
- 抽象方法名 : test
- 方法描述 : 处理T类型参数,返回boolean值
- 其他方法 : and,or,negate,isEquals
- 补充知识点 : 
    - 提供了一个对应的非泛型接口: `IntPredicate`,参数为int类型时建议使用,减少执行时的拆装箱 
    - 提供了处理两种类型参数的接口: `BiPredicate<T,U> `

#### Predicate<T>接口示例: 

- Predicate接口常用于为集合对象执行筛选条件
    - 这次示例,使用`BiPredicate<T,U>`接口
    - 示例作用是,根据年龄筛选出列表中的人 

```
public static void main(String[] args) {
    List<Person> people = new ArrayList<>();
    people.add(new Person("Killua",11));
    people.add(new Person("Netero",110));
    people.add(new Person("alluka",10));
    people.add(new Person("Zeno",67));

    usingLambda(people);

    NotUsingLambda(people);
}

// 使用了Lambda表达式的方法
private static void usingLambda(List<Person> people) {
    BiPredicate<Person, Integer> judgeByAge = (p, i)->p.getAge()>i;

    people.forEach(person -> {
        if(judgeByAge.test(person,60))
            System.out.println(person.getName() + ": is a old man");
        else
            System.out.println(person.getName() + ": is a young man");
    });
}

// 未使用lambda表达式的方法
private static void NotUsingLambda(List<Person> people) {
    BiPredicate<Person, Integer> judgeByAge = new BiPredicate<Person, Integer>() {
    @Override
    public boolean test(Person person, Integer i) {
        if (person.getAge()>i)
            return true;
        else
            return false;
    }
};

    for (Person person : people) {
        if(judgeByAge.test(person,60))
            System.out.println(person.getName() + ": is a old man");
        else
            System.out.println(person.getName() + ": is a young man");
    }
}
```

### Function<T,R>接口 

#### Function<T,R>接口概述: 

- 接口名 : Function<T,R>
- 参数类型 : T
- 返回值类型 : R
- 抽象方法名 : apply
- 方法描述 : 处理T类型参数,返回R类型参数
- 其他方法 : compose,andThen,identity
- 补充知识点 : 
    - 提供了一个对应的非泛型接口: `IntFunction`,参数为int类型时建议使用,减少执行时的拆装箱 
    - 提供了处理两种类型参数的接口: `BiPredicate<T,U,R>`,表示传入参数是T,和U类型,返回值R类型 

#### Function<T,R>接口示例: 

- 示例说明:
    - 示例是,传入一句话,算出里面多少个单词 

```
String test = "This is a word-count test";
Function<String,Integer> wordCount =
        s -> s.split(" ").length;
System.out.println(wordCount.apply(test));
```

至此,完...... 
> 
*RIP Dolores O`Riordan* 

<div align="center">![Wake Up And Smell The Coffee][3]</div>

<!-- 参考文献 -->
[1]: https://docs.oracle.com/javase/8/docs/api/ "Java8 API"
[2]: http://storage.live.com/items/AEE68C12565C1619!203841:/java_basic05_Lambda01.png?authkey=AJoh90nl3u6Wj4U
[3]: http://storage.live.com/items/AEE68C12565C1619!203843?authkey=AJoh90nl3u6Wj4U

