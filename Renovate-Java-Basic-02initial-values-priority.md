---
title: Java基础重修_02字段初始化的处理步骤&代码块小结
date: 2018-01-04 11:57:50
update: 2018年1月4日 12点15分
tags: 
    - 笔记
    - Java
    - 基础知识
categories: 
    - 笔记
    - Java
---
## 写在之前的话
- 本系列主要记录一些Java基础的难点,参杂一些Java8的新特性
	- 主要是杜若汇集[<Java 核心技术 卷一>][2]中遇到的难点和问题
	- 适合掌握了Java基础知识的同学进行重点和难点的复习 

呐~正文开始啦~🐱‍👤
## 字段初始化的处理步骤
- 一个类中,初始化字段的方法有很多种
	1. 声明后即定义值
	2. 初始化构造代码块
	3. 静态代码块
	4. 构造方法
	5. 调用方法赋值,eg:(setXxx方法)
- 这些方法的优先级如何呢
- 我们可以设计代码来观察一下: 

```
// 总结:
	// 下面的代码及不严谨,只是为了验证赋值顺序才这样操作的
		// 实际开发中要避免这样重复赋值的情况
		// 里面对静态变量的调用方式也不可取
	// 请结合总结,使用注释控制初始化条件,观察测试方法的结果
public class InitializeBlockTest {
	public static void main(String[] args) {
		InitialBlockClass test1 = new InitialBlockClass();
		test1.setField("set方法赋值");
		System.out.println(test1.getField());
		InitialBlockClass test2 = new InitialBlockClass("有参构造赋值");
		System.out.println(test2.getField());
	}
}

class InitialBlockClass {
	// 因为调用了静态代码块,所以成员变量使用了静态的
	private static String field="设定初始值";
	
	public static String getField() {
		return field;
	}

	public static void setField(String field) {
		InitialBlockClass.field = field;
	}
	
	static {
		field="静态代码块";
	}
	
	{
		field="初始化构造代码块1";
	}
	
	public InitialBlockClass(String field) {
		this.field = field;
	}
	
	public InitialBlockClass() {
		field="空参构造赋值";
	}
	
	{
		field="初始化构造代码块2";
	}
}
```
### 初始化字段的处理步骤:结论
- 通过以上代码,可以得出以下初始化代码的处理步骤(优先级由高到低)如下:
	1. 初始化的默认值,即声明后定义的值,对应代码示例的:
		- `public static String field="设定初始值";`
	2. 静态代码块中的初始化方法,对应示例代码的:
		- `static { field="静态代码块"; }`
	3. 依据类中的顺序,执行的初始化构造代码块,对应示例代码的:
		- `{ field="初始化构造代码块1 }";`,`{ field="初始化构造代码块2"; }`
	4. 执行对应的构造器方法
		- 如果构造器方法种调用了其他构造器方法,执行内部调用的方法进行初始化
	5. 调用方法赋值,对应示例代码的:
		- `test1.setField("set方法赋值");`

## 静态代码块和构造代码块区别 


### 代码块概述
- 代码块是由大括号包裹的代码段`{ ... }`;
- 其主要作用是限制代码的有效范围
	- 即,代码块内部定义的参数/方法,在代码块外部无法直接获取
- 代码块分类:
	- 类中方法外:
		- 静态代码块
		- 构造代码块
	- 方法中:
		- 普通代码块
			- 调用时,普通代码块依据代码顺序依次执行 

### 静态代码块和构造代码块
- 构造代码块
	- 类中方法外定义的,用`{}`括起来的内容,示例见上段代码
- 静态代码块
	- 类中方法外定义,在构造代码块前面加`static`关键字
- 区别:
	- `构造代码块`在每次创建类对象时都会被调用,执行次序优先于类中其他方法
	- `静态代码块`只有类首次被创建时才执行调用,执行次序优先于构造代码块
	- `静态代码块`只能直接访问类中的静态方法和静态变量 

至此,完......
<div align="center">![Nujabes_Luv][1]</div>

<!-- 参考文献 -->
[1]: http://storage.live.com/items/AEE68C12565C1619!199126?authkey=AJoh90nl3u6Wj4U "Album Cover Nujabes_Luv"
[2]: http://product.dangdang.com/24035306.html "Java 核心技术 卷一"