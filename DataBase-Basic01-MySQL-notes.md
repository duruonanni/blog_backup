---
title: MySQL 笔记整理
date: 2018-01-24 21:08:35
update:
tags:
    - 笔记
    - 基础知识
    - 数据库
    - MySQL
categories:
    - 笔记
    - 数据库
---

## 笔记说明:
这是杜若初学MySQL时记录的笔记,现重新整理并发布. 

## SQL(Structured Query Language)概述

### SQL介绍
- 定义: 
	- SQL,结构化查询语言,是关系型数据库管理系统都需要遵循的规范; 
	- 在与数据库进行交互时,所使用的特殊的语言,叫SQL,他是数据库的代码;  
	- 不同的DBMS厂家生产的软件都支持SQL语言,但是都具有特有语句(方言);  
		- 除了SQL标准语句外,大部分的SQL数据库程序都拥有它们的私有扩展,这是方言形成的原因;  
- 本次使用的DBMS是开源的[MariaDB](https://mariadb.org/);
    - MariaDB是MySQL的开发者Widenius离开Sun公司后,重新开发的,兼容MySQL的数据库;
    - 在基本的使用上MairaDB和MySQL差别不大;

### SQL语句分类
- 分类目的: 
	- 分类之后,更方便理解和记忆;  
- 具体分类: 
	- DDL(Data Definition Language) : 数据定义语言
		- 用于定义数据库对象: 数据库,表,列;  
		- 简言之: 生成数据库;
		- 关键字: create,alter,drop...;
	- DML(Data Manipulation Language) : 数据操作语言
		- 用于对数据库中表的记录的进行更新;  
		- 简言之: 增删改;
		- 关键字: insert,delete,update...
	- DQL(Date Query Language) : 数据查询语言(**难点**)
		- 用于查询数据库中表的记录
		- 关键字: select,from,where...
	- DCL(Data Control language) : 数据控制语言
		- 用来定义数据库的访问权限和安全级别,创建用户;  
		- 主要是DBA(DataBase Administer)操作;  
		- 此次不学习
		
### SQL通用语法(格式的注意事项)
- SQL语句可以单行或者多行书写,以分号`;`结束(同Java);  
- 可使用空格和缩进和换行来增强语句的可读性;  
- MySQL数据库的SQL语句不区分大小写,关键字建议使用大写;  
	- 自己定义的名称,全部使用小写,单词和单词之间使用下划线;  
	- eg : `SELECT * FROM user`;
- 除了具体数据和注释外,SQL中最好不要出现中文;  
- 同样可以使用`/* */` 的方式完成多行注释; 
	- 也可以写`# 后面写注释`(用于单行注释);  
	- 也可以使用`-- 后面写注释`(用于单行注释);
- MySQL中的我们常使用的数据类型如下: 
	- `int` : 整型
	- `double` : 浮点型
	- `varchar` :  字符串型(variable character)(相当于Java的String)
	- `date` : 日期类型
		- 日期类型格式为: yyyy-MM-dd.只写年月日;  
	- 此外还有更细致的数据类型,这里不再介绍;  
	
### MySQL客户端基本使用介绍
- 客户端软件说明: 
	- 通常情况下,我们可以在CMD中使用语句对我们的MySQL数据库进行控制.但是,在其中我们无法保存书写的SQL语句;  
	- 为此,我们使用具备图形界面的MySQL客户端执行SQL代码.这样,不知能保存我们书写的SQL语句,还能在书写时实现代码补全,高亮等功能,更方便我们使用;  
	- 目前,市面上的相关软件很多,我是用的是`Navicat`.因为其对mariaDB的友好.
        - 在Win上,安装MairaDB时,会附赠安装`HeidiSQL`;
        - `HeidiSQL`缺少自动补全和提示的操作,所以我更愿意使用更熟悉的`Navicat`;
- 软件使用说明: 
	- 此软件自带中文版,正常安装即可;
	- 软件自身支持代码补全,补全快捷键`Tab`;  
	- 软件本身支持关键字识别,可以自动将关键字转成大写;  
	- 写好代码后,按`Ctrl+R`可顺序执行所有代码;
    - 选中需要运行的代码,按`Ctrl+R`,只运行选中的代码;
	
### 小知识1: CMD中文乱码
- 问题描述: 
	- 在CMD中,使用MySQl进行中文操作时,会出现乱码; 
- 问题原因: 
	- mysql的客户端设置编码是utf8,而系统的cmd窗口编码是gbk;  
- 解决办法1: 让操作的数据以gbk编码进行存储(不推荐)
	- 在操作MySQL时,CMD中输入`set names gbk;`
	- 让MySQL的客户端也使用gbk编码
- 解决办法2: 不要在CMD中进行中文操作
    - 使用MySQL的客户端进行所有操作,确保存储时的编码和取用时都是utf8

---

## DDL--数据库定义
- 创建数据库
	- 语法: 创建指定名称的数据库,默认的编码方式是`UTF-8`;  
		- `CREAT DATABASE 数据库名;`  
		- `CREAT DATABASE 数据库名 CHARACTER SET 字符集;`  
		
- 查看数据库
	- 语法1: 显示当前服务器中的所有数据库;  
		- `SHOW DATABASES;` 
			- 注意单词是复数形式;  
	- 语法2: 显示某个数据库的定义信息,包括数据库名和编码方式;  
		- `SHOW CREATE DATABASE 已存在数据库名;`
		- 如果写的数据库名不存在,执行报错;  
		
- 删除数据库 
	- 语法1 : 删除选中的数据库
		- `DROP DATABASE 数据库名称;`  
	- 说明1 : 就算数据库中有数据也是直接删除;
		
- 使用数据库
	- 语法1 : 查看当前使用的数据库
		- `SELECT DATABASE();`
		- 注意小括号`()`;
	- 语法2 : 切换到某个数据库
		- `USE 数据库名;`
		- 切换不存在,执行报错;  
		
### DDL--表结构操作(上): 表的操作
- 创建表
	- 语法如下:  
```
CREATE TABLE 表名 (
	字段名 类型(长度) [约束],
	字段名 类型(长度) [约束]
	...
);
```
- 创建表语法说明: 
	- 表中字段被用`()`包起来,末尾以`;`结束;  
	- 写完一行字段需要在后面加上`,`,如果是最后一行字段,不能加`,`;  
	- 在语法中用`[]`标记的表示可以不加的语句;  
		- 此处的约束详见下面`约束详解`;  
	- 如果类型是`DOUBLE`,需要限定小小数点后面的个数,见如下示例: 
		- `student_ch DOUBLE(3,2),`
		
- 查看表
	- 语法1 : 查看当前数据库下的所有表: 
		- `SHOW TABLES;`
	- 语法2 : 查看表结构:
		- `DESC 表名;` (describe)
		- 会显示表中的字段,类型(长度),是否为主键,默认值等;  

- 删除表: 
	- 语法 : 删除指定表
		- `DROP TABLE 表名;`  
	- 说明: 
		- 删除的表不存在,运行报错; 
		
### 小知识2: drop,truncate和delete的区别
- 说明: 
	- 三者都有删除的意思,但使用范围,删除结果,语法等都各有不同;  
- DROP: 
	- 应用范围: 
		- 删除数据库,删除表,删除表中的列,删除约束; 
	- 删除结果: 
		- 删除整个表格
	- 语法示例: 
		- 删除数据库: `DROP DATABASE 数据库名;`
		- 删除表(表中的数据): `DROP TABLE 表名 where 条件;`
		- 删除表中的列(字段): `ALTER TABLE 表名 DROP 列名;`
		- 删除约束: 略(详见下面`约束`);  
- DELETE: 
	- 应用范围: 
		- 删除表中的数据,不释放空间,不删除定义;
	- 删除结果: 
		- 删除表格时,只删除表中数据,表格中索引不会被删除;  
		- 对具备`AUTO_INCREMENT`的值,删除后添加数据仍按原值递增;  
	- 语法示例: `DELETE FROM 表名 [WHERE 条件];`
- TRUNCATE: 
	- 应用范围: 
		- 删除表中的数据,释放空间,但不删除定义
	- 删除结果: 
		- 删除整个表,并删除相关索引,再创建一个同名表格;  
		- 对`AUTO_INCREMENT`结果清零;  
	- 语法示例: `TRUNCATE TABLE 表名;`
	- 此外,truncate还可以限定double类型的小数显示位数;
        - 示例见下面: [小知识3: truncate](#truncate)

### 小知识3: MySQl中显示指定位数的小数
- 概览: 
	- MySQL中可使用多种函数控制double类型数据的显示样式(就是小数点后面的位数)
    - 下面是常用的函数: 
1. formate: 
    - 说明: 
        - 对返回值进行四舍五入
        - 返回值变为字符串类型
        - 显示的数字满三位会添加一个逗号`,`
    - 语法: 
        - `formate(param,number);`
    - 语法示例: 
        - `SELECT format( 314159.14159,4 );`
        - 结果: 
            - 314,159.1416
2. <span id="truncate">truncate</span>:
    - 说明:
        - 返回值不进行四舍五入,直接截断
        - 返回值类型不变
    - 语法: 
        - `truncate(param,number)`
    - 语法示例: 
        - `SELECT truncate( 314159.14159, 4 );`
        - 结果: 
            - 314159.1415
3. round:
    - 说明: 
        - 返回值进行四舍五入
        - 返回值类型不变
        - 可使用 `round(param)`,四舍五入掉所有小数
    - 语法1: 
        - `round(param,number)`
    - 语法2: 
        - `round(param)`
        - 四舍五入掉所有小数
    - 语法示例: 
        - `SELECT round( 314159.14159, 4 );`
        - 结果: 
            - `314159.1416`
4. convert:
    - 说明: 
        - convert函数的第二个参数,不是一个数字,而是一个`decimal`函数
            - `decimal(10,2)` : 表示显示一个8(10-2)位的数字,小数保留到后2位
        - 返回值进行四舍五入
    - 语法: 
        - `convert(param,decimal(x,y))`
    - 语法示例: 
        - `SELECT convert( 314159.14159, DECIMAL ( 10, 4 ) );`
        - 结果: 
            - `314159.1416`
    
### DDL--表结构操作(下) : 字段的操作
- 修改表结构格式: 
	- 语法1: 为表添加一个新的字段列 
		- `ALTER TABLE 表名 ADD 列名 类型(长度) [约束];`  
			- 添加的表名不能和已有的相同(就算类型不同也不行); 
			- 添加的列名建议用重音符括起来(上排数字1左边的按键);  
            - 约束的说明,详见[此处](#constraint);
	- 语法2: 修改列的类型长度及约束
		- `ALTER TABLE 表名 MODIFY 列名 类型(长度) [约束];`  
			- 修改列名不能原表中没有; 
			- 在表中存有数据时: 
				- 无法更改数据类型; 
				- 可以更改数据长度,但长度不能比已有数据的值短;  
			- 可以把修改的写成和之前列一致(没什么用,但不报错);  
	- 语法3: 修改列名;  
		- `ALTER TABLE 表名 CHANGE 旧列名 新列名 类型(长度) [约束];`  
			- 新列名不能是已有列名;  
	- 语法4: 删除列
		- `ATLER TABLE 表名 DROP 列名;`
			- 删除不存在的列报错; 
			- 即使列中有数据,也可以顺利删除;  
	- 语法5: 修改表的字符集(编码方式)
		- `ALTER TABLE 表名 CHARACTER SET 字符集;`
	- 语法6: 修改表名
		- `RENAME TABLE 表名 TO 新表名;`

---

## <span id="constraint">SQL约束(constraint)</span>
- 约束的作用: 
	- 用于限制加入表的数据类型,默认值等;  
- 规定约束的位置: 
	1. 创建表时(使用`CREATE TABLE`语句); 
	2. 更改表内容时(使用`ALTER TABLE`语句); 

### 约束的分类: 
1. `NOT NULL` (非空约束)
	- 受到非空约束的字段,不能接受`NULL`值;
	- 这意味着,添加数据时,如果不为该字段添加非空值,就无法插入或更新记录;  
  
2. `UNIQUE` (唯一约束)
	- 受到唯一约束的字段,存储的数据不能重复;  
	
3. `PRIMARY KEY` (主键约束)
	- 一张表中,只允许存在一个主键约束的字段(也可以没有); 
		- 可以`在创建表时`将多个列集合存储约束为一个主键,具体语法示例如下: 
			- `CONSTRAINT 表名 PRIMARY KEY(属性1名,属性2名);`  
            - 这叫做**联合主键**
	- 受到主键约束的字段,同时受到`非空`和`唯一`约束; 
		- 其他字段也可以自行添加`非空`和`唯一`约束,但`主键约束`只能有一个;  
	- 取消主键约束后,该字段仍然保留非空约束,但不再有唯一约束;  
	
4. `AUTO_INCREMENT` (自动增长约束)
	- 此约束只有在创建表时候添加;
	- 该约束只对`整型`的字段有效,只能对`KEY`添加自动增长(主键或外键,通常给主键添加);  
	- 在未设置默认值的情况下,受到此约束的字段值默认为1,且每添加一次此字段值就加1;  
		- 即使使用了`DELETE`删除了表数据,在添加新数据时,仍会接着之前的增长值进行添加;
			- 因为这样并不会影响表的结构,属性和索引;  
		- 只有使用了`Truncate`删除表数据后,此约束的记录才会被重置;  
			- 实质是删除了整张表再重建该表,索引被重置了;  
	- 只更新表的数据不会触发自增;
	- 有自增的字段自行添加了数值后,再次自增是从之前自增的值继续+1
	- 给添加了自加的字段添加NULL值,会出现什么情况?
		- 可以被添加进去,会将空值改成自加值;  
		
5. `FOREIGN KEY` (外键约束)
	- 外键约束用于多表操作中,详见后面[多表操作](#mutitable);  
	
6. `CHECK` (取值约束)
	- 受到取值约束的字段,只能创建符合约束的值;
		- eg : `Id_P int NOT NULL CHECK (Id_P>0)` (ID_P 的值必须大于0)
		
7. `DEFAULT` (默认值约束)
	- 受到约束的字段,具备默认值;
	- 添加或更新数据时,该字段未规定其他值,会赋予其默认值; 
	- 在未加以限定时,数据默认值都是`NULL`(除`主键`外);  
		- 默认值用单引号`'...'`引起来;  
	- 给具有自增约束的元素添加默认值约束,会出现严重错误:
		- 该元素不再具备自增效果,未赋值情况下多次添加,都使用默认值;  

### 约束的创建方式: 
- 说明: 
	- 约束的创建方式有三种,以下以`PRIMARY KEY`举例:  
1. 创建表时,在字段中指明约束:  
	- 语法: 
		- 直接在需要添加约束的字段后面添加约束;  
	- 说明: 
		- 可以一次给一个字段添加多种约束;
		- 此方法不能在约束前添加`CONSTRAINT 约束名`;
		- 此方法不能将多列集合约束到一个约束;  
	- 特殊说明: 
		- 对`DEFAULT`默认值约束,其语法如下: 
			- `DEFAULT '默认值';`
		- 对`CHECK`取值约束,其语法如下: 
			- `CHECK (约束要求);`
			- 约束要求如 字段值>0;  
```
CREATE TABLE people
(
	# 定义字段时指明约束
	Id_P int PRIMARY KEY, 
	LastName varchar(255),
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255)
);
```
  
2. 表格创建后,在最后添加约束语句: 
	- 语法(写在创建表格内的最后一句): 
		- `[CONSTRAINT 约束名] PRIMARY KEY (字段列表)`
	- 说明: 
		- `[CONSTRAINT 约束名]`可省略;
			- 通常情况下,我们将约束名命名为: 
			- 约束名简写_字段名,eg : `pk_pid`
			- 约束名称并没有什么用;  
		- 字段列表可添加多个字段,这样多个字段合体接收约束;
			- eg1: 字段(名和姓)受到唯一约束,只有姓名都一致的值重复才受到约束; 
			- eg2: 两个以上字段受到主键约束,被称为`联合主键`;  
```
CREATE TABLE people
(
	FirstName varchar(255),
	LastName varchar(255),
	Address varchar(255),
	City varchar(255),
	PRIMARY KEY (FirstName,LastName)
)
```
  
3. 表格创建后,通过修改表结构,声明指定字段的约束: 
	- 语法(单独成句): 使用`ADD`关键字
		- `ALTER TABLE people ADD [CONSTRAINT 约束名] PRIMARY KEY (字段列表);`
	- 说明: 
		- 字段列表的小括号`()`不能少,就算只有一个字段也不能少;  
		- 添加过约束的字段可以被重复添加,因此也可以使用此语法来`修改约束`;  
	- 特殊说明: 
		- 对`DEFAULT`默认值约束,其语法如下: 
			- `ALTER TABLE people ALTER 字段名 SET DEFAULT '默认值';`
	
### 约束的删除方式: 
- 语法: 
	- `ALTER TABLE 表名 DROP 约束名 约束字段;`
- 说明: 
	- 由于主键约束的唯一性,所以删除主键约束时不需要写约束字段;  
- 特殊说明: 
	- 对`DEFAULT`默认值约束,其语法如下: 
		- `ALTER TABLE people ALTER 字段名 DROP DEFAULT;`
	- 对`UNIQUE`唯一约束,其语法如下:
		- `ALTER TABLE people DROP INDEX 字段名;`
    - 对`PRIMARY KEY`主键约束,其语法如下:
        - `ALTER TABLE people DROP PRIMARY TABLE`

---

## DML数据操作语言(增删改)
- 前言 : 
	- 所操作的数据,除了数值类型外,都必须使用分号`'...'`括起来;  
	- 为了方便统一,可以将所有的值都用`'...'`括起来;  
	- 写入的字段值不能超过最大值,否则只能接收字段值范围的值;  
	
- 插入表记录: `INSERT`
	- 语法1: 向表中插入某些字段
		- `INSERT INTO 表名 (字段1,字段2..) value (值1,值2,值3);`  
			- 向表中插入的字段和值必须一一对应;  
	- 语法2: 向表中插入所有字段
		- `INSERT INTO 表名 values (值1,值2...);`
			- 必须补全表中所有字段,顺序也不能变; 
			- 插入空,可以不写,或者写`NULL`; 
	
- 更新表记录: `UPDATE`
	- 语法1: 更新所有记录的指定字段
		- `UPDATE 表名 SET 字段名1=值1,字段名2=值2,...;` 
	- 语法2: 更新符号条件记录的指定字段
		- `UPDATE 表名 SET 字段名=值,... WHERE 条件;`  
		- 条件可以是指定原字段的值是哪个时,进行更新; 
	
- 删除表记录: `DELETE` & `truncate`
	- 语法1: 删除表中所有的记录,不清空`AUTO_INCREMENT`;
		- `DELETE FROM 表名 [WHERE 条件];`
	- 语法2: 删除整个表,再重建同名空新表,清空`AUTO_INCREMENT`;  
		- `TRUNCATE TABLE 表名;`
	
--- 

## DQL简单查询和条件查询
- 准备工作
	- 准备一个表格
- 语法  
    - `SELECT [DISTINCT] *|列名1,列名2 from 表名 where 条件`
- 语法说明: 
	- 使用了关键字`DISTINCT` 后查询出的结果是去重的;  
	- 竖线代表要么使用`*`查询所有列,要么使用具体列名进行查询;  
	
### 条件查询关键字

#### 条件运算符
- 作用: 
	- 执行条件判断
- 语法1: `AND` 
	- 和运算符,多个条件同时满足;
- 语法2: `OR`
	- 或运算符,多个条件成立一个即可;  
- 语法3: `NOT`
	- 非运算符,条件不成立即可;  
- 说明: 
	- 除了`NOT`,另外两个运算符都不能单独出现;  
	
#### 比较运算符
- 语法1: `> < <= >= = <>`
	- 大于 小于 大于(小于) 等于 不等于
	- 在MySQL中,不等于`<>` 也可以写作 `!=`  
- 语法2: `BETWEEN...AND...`
	- 在某个区间内(在MySQL中包含头尾值) 
	- 查询的值既可以是数字,也可以是文本的字典序
- 语法3: `IN(具体值1,具体值2...)`
	- 和具体值相同的值
- 语法4: `LIKE '%_'`
	- 模糊查询,添加模糊关键字进行查询
		- 其中`%`代表零或多个任意字符;
		- 其中`_`代表一个任意字符;
		- 也可添加`[charlist]`
			- 代表满足`[]`中任一单一字符;
			- `[^]`或`[!]`代表不在字符列中的任一单一字符;  
	- 通过模糊关键字制造类似正则的查询方式;  
	- 注意: 
		- 模糊查询效率很低,在数据很多时慎用;  
- 语法5: `IS NULL`
	- 判断是否为空;  
	
### 别名查询AS
- 说明: 
	- 表别名: 
		- 在执行多表查询时,对查询条件进行限制时,通常需要将字段写做`表名.字段名`的形式;  
		- 对表名使用别名,可简化在限定时的代码量,也让代码阅读更容易;
	- 列别名: 
		- 在执行查询时,结果字段不够说明显示情况,可使用列别名重命名字段名;  
- 语法: 
	- `SELECT * FROM 表名 [AS] 表别名;`
	- `SELETC 字段名 [AS] 列别名 FROM 表名;`
- 语法说明: 
	- 其中的`AS`可以省略; 

### 简单查询和条件查询语法示例:  
```
-- 1.查询所有的商品
SELECT * FROM product;
-- 2. 查询商品名和商品价格
SELECT pname, price FROM product;
-- 3. 别名查询.使用关键字as
-- 表别名
	-- 别名在多表关联时候使用
SELECT * FROM product AS pro;
-- 列别名
SELECT pname AS n,price AS p FROM product;
SLECT DISTINCT price FROM product;
-- 4. 查询结果是表达式(运算查询): 可以对商品价格进行加价
-- 只是显示变了,对值没有改变
SELECT 100 + 21;
SELECT pid, price+200 AS bad_price FROM product;
-- 5. 比较运算符
SELECT * FROM product WHERE price > 400;
-- 查询价格在1000到4000之间的记录
SELECT * FROM product WHERE price >= 1000 AND price <= 4000;
SELECT * FROM product WHERE price BETWEEN 1000 AND 4000;
-- 查询价格是200或800或440的记录
SELECT * FROM product WHERE price=200 OR price=800 OR price=440;
SELECT * FROM product WHERE price IN (200, 800, 440);
-- 6.模糊查询 (_代表一个任意字符,%代表任意个任意字符)
# 模糊查询的效率比较低,尽量少用
SELECT * FROM product WHERE pname LIKE '花花__';
SELECT * FROM product WHERE pname Like '%花%花%';
```

---

## SQL单表查询优化
- 概述: 
	- 之前讲解了DQL中的简单查询和条件查询,有了这两种方式,我们就能方便的找到数据库中我们需要的内容;  
	- 但是,查询结果较多的话,我们需要将结果进行整理统计.这就要用到下面的语句了: 

### 查询结果_排序(ORDER BY)
- 说明:
	- 通过`ORDER BY`语句,可以对查询结果进行排序;
		- `ASC` Ascend 升序 (默认)
		- `DESC` Descent 降序
			- 注意和查看表结构时使用的`DESC 表名;`区分
			- 单词简写相同...
		- 数据是数值类型,直接对比大小;
		- 数据非数值类型,对比字典序大小;
		- 多个数据排序,以`,`区分,靠前面的为主,后面的为辅;
- 语法: 
	- `SELECT * FROM 表名 ORDER BY 字段名 ASC | DESC;`
	- 可添加`DISTINCT`对结果去重;  
	
### 查询结果_聚合
- 说明: 
	- 使用聚合函数,可以对查询出的数值进行统计计算,返回一个直观的单一值;  
- 聚合函数: 
	- `COUNT` : 统计指定列的个数(不包含NULL值的);  
	- `SUM` : 统计指定列的数值和; 
	- `AVG` : 统计指定列的平均值;
		 - 以上两个如果指定列不是数值类型,返回0;  	
	- `MAX` : 获得指定列的最大值;  
	- `MIN` : 获得指定列的最小值; 
		- 如果指定列是字符串类型,输出字符串的排序结果;  
- 语法: 
	- `SELECT 聚合函数(* | 字段名...) FROM 表名;`
	- 可通过`WHERE`对结果进行约束;  
	
### 查询结果_分组
- 说明: 
	- 通过`GROUP BY`语句,可以对查询结果进行分组;  
- 语法: 
	- `SELECT 字段1... FROM 表名 GROUP BY 分组字段 [HAVING 分组条件];`
- 语法说明: 
	- 通过数据中分组字段的值进行分组;
	- 添加`HAVING`语句,实现对分组后的条件进行过滤,类似`WHERE`;  
- `HAVING`和`WHERE`的区别: 
	 - `HAVING`是在分组后对数据进行过滤;
		- `WHERE`是在分组前对数据进行过滤;
	- `HAVING`后面可以使用聚合函数;
		- `WHERE`后面不能使用聚合函数;

### 截取
- 说明:
	- 在查询数据时,通过`LIMIT`关键字,可限定显示结果的数目;  
- 语法: 
	- `SELECT 字段1... FROM 表名 LIMIT 显示结果的个数;`
- 语法说明: 
	- 限制后,只显示结果的前n个;  
		
-----------

## <span id="mutitable">多表操作</span>

### 多表操作概述: 
- 定义: 
	- 在同一个数据库中,可以具备多个表.让表与表之间的数据产生联系,可以通过多表操作实现;  
- 说明: 
	- 通过在表与表之间缔结关系实现多表操作;  
	- 常见的多表关系是:`一对多关系`,`多对多关系`;  
		- 一对一关系的,直接合并成一张表更方便;  
	- 表与表之间通过`外键`(FOREIGN KEY)建立关系;  

- 一对多
	- 概述: 
		- 一个主表中的数据提供给多个从表中的数据使用;  
		- 常见实例: 客户和订单,分类和商品,部门和员工;  
	- 原则: 
		- 在从表创建一个字段,该字段作为`外键`指向主表的主键;  

- 多对多
	- 概述: 
		- 两个个表之间通过一个中间表实现多对多关系;  
		- 常见实例: 学生和课程;  
	- 原则: 
		- 需要创建第三张表,第三张表中间至少两个字段,这两个字段分别作为`外键`指向各自一方的主键;  
		
### 外键约束详解
- 外键概述
	- 表与表之间,通过外键建立联系; 
	- 从表外键的值是对主表外键的引用;  
	- 从表外键类型,必须与主表主键类型一致;  
- 外键作用: 
	- 在添加数据时,对数据进行约束;
	- 在查询数据时,可在多表查询的交叉连接或内链接时,添加主表主键值等于从表外键值达到去除重复的作用;  
- 声明语法:
	- `ALTER TABLE 从表 ADD [CONSTRAINT 外键名称] FOREIGN KEY (从表外键字段名) REFERENCES 主表 (主表主键);`
- 声明语法说明: 
	- 外键名称 通常写做 `字段名_fk`; 
	- 使用从表外键关联主表主键;
- 删除语法: 
	- `ALTER TABLE 从表名 DROP FOREIGN KEY 外键名称 | 字段名;`
- 删除语法说明: 
	- 如果声明了外键名称,只能使用外键名称删除;  
	- 如果未声明外键名称,使用对应字段名删除;  
	
### 自连接操作
- 定义: 
	- 想象我们通过数据库实现省与市之间的联系.一个省对应多个市,这可以通过外键约束实现一对多联系.此外,各个省市还拥有自己的名字(NAME),概述(DESCRIPTION).经过分析,我们可以对这个要求创建如下表格:   
```
CREATE TABLE AREA(
	id VARCHAR(32) PRIMARY KEY,
	NAME VARCHAR(50),
	description VARCHAR(100),
	parent_id VARCHAR(32)
);
```  

----------

## 多表查询

- 定义: 
	- 在MySQL中,可实现多表查询.即将多个表中的内容通过查询一起显示出来;  
	- 多表查询的表格之间通常具备外键约束,目的时方便去除冗余内容.但是,不具备外键约束的表格之间也可以实现多表查询;  
		- 因为外键约束主要在添加数据时起约束作用;  
	- 多表查询可使用`交叉查询`或者`内链接`;  

### 交叉连接(笛卡尔积)
- 定义:  
	- 交叉查询,得到两个表的乘积;
- 语法: 
	- `select * from A,B;`
- 说明: 
	- 写在前面的(A)是主表,后面的(B)是附表;  
	- 默认显示时,B中的一个列,对应主表中的所有列显示;
	- 得到的总数据为: A列数*B列数;  
		- 行数是A行数+B行数;  
	- 这样的结果很冗余,所以通常通过有效的条件获得我们想要的显示结果;  
		- eg: 具备外键约束时,通过`主表主键=从表外键`去除多余列;  

### 内链接查询
- 定义: 
	- 就是给交叉查询添加了条件,让查询值变得更便于阅读;  
	- 在两个表中,只要有一个表存在数据,即使对应表没有数据,仍然显示该行(无数据处显示空白);  
- 语法: 
	- 隐式内连接: 
		- `SELECT * FROM A,B WHERE 条件;`
	- 显示内连接: 
		- `SELECT * FROM A [inner] JOIN B ON 条件;`
		- 说明: 
			- `INNER`可省略 

### 外连接查询
- 定义: 
	- 和内连接相似,区别见下`语法说明`;  
- 语法: 
	- 左外键连接
		- `SELECT * FROM A LEFT [OUTER] JOIN B ON 条件;`
	- 右外键连接
		- `SELECT * FROM A RIGHT [OUTER] JOIN B ON 条件;`
	- 全键连接:
		- `SELECT * FROM A FULL [OUTER] JOIN B ON 条件;`
- 语法说明: 
	- 在左外键连接中,即使右表中没有匹配的条件,结果任然会返回,只是相应字段为空;  
	- 右外键连接同理;  
	- 全连接会返回所有结果,无论是否有对应存在;  

### 子查询
- 定义: 
	- 将一条SELECT查询语句作为另一条SELECT语句中的一部分(查询条件,查询结果等)
- 说明: 
	-  子查询不能做字段;
	
### 联表查询UNION
- 定义:   
	- 两张表,如果字段相同,只是存储的数据不同,可通过联表查询一起读出结果;  
- 语法:   
	- `SELECT * FROM 表名1 UNION SELECT * FROM 表名2;`
	- `SELECT * FROM 表名1 UNION ALL SELECT * FROM 表名2;`
- 语法说明: 
	- 必须要满足所查询的字段类型相同长度相同(名字可以不同);  
		- 两张表的其他字段可以不同;  
	- `UNION`和`UNION ALL`的区别是:
		- 前一个会执行去重; 
		- 后一个不执行去重;  