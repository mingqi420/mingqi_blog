---
title: "Go语言和其他语言的差异"
subtitle: ""
date: 2022-05-30T15:18:45+08:00
lastmod: 2022-05-30T15:18:45+08:00
draft: false
toc:
enable: false
weight: true
categories: ["程序设计"]
tags: ["go"]
---
# 	Go语言和其他语言的不同

​	Go语言作为出现比较晚的一门编程语言，在其原生支持高并发、云原生等领域的优秀表现，像目前比较流程的容器编排技术Kubernetes、容器技术Docker都是用Go语言写的，像Java等其他面向对象的语言，虽然也能做云原生相关的开发，但是支持的程度远没有Go语言高，凭借其语言特性和简单的编程方式，弥补了其他编程语言一定程度上的不足，一度成为一个热门的编程语言。

​	最近在学习Go语言，我之前使用过C#、Java等面向对象编程的语言，发现其中有很多的编程方式和其他语言有区别的地方，好记性不如烂笔头，总结一下，和其他语言做个对比。这里之总结差异的地方，具体的语法不做详细的介绍。

​	种一棵树最好的时间是十年前，其次是现在。

## 基础语法

### 1、变量

​	1）Go语言变量声明语句不需要使用分号作为分割符

```go
var v1 int
var v2 string
```

​	2）可以将若干个需要声明的变量放在一起

```go
var(
	v1 int
  v2 string
)
```

​	3)变量初始化时候可以和其他语言一样直接在变量后面加等号，等号后面为要初始化的值，也可以使用变量名：=变量值的简单方式

```go
var v1 int=10//正常的使用方法
var v2=10//编译器可以推导出v2的类型
v3:=10//简单写法 明确表达同时进行变量声明和初始化工作
```

​	3）变量赋值  Go语言的变量赋值和多数语言一致，但是Go语言提供了多重赋值的功能，比如下面这个交换i、j变量的语句：

```go
i,j=j,i
```

​	在不支持多重赋值的语言中，交换两个变量的值需要引入一个中间变量：

```java
t=i；i=j；j=t;
```

​	4)匿名变量

​		在使用其他语言时，有时候要获取一个值，却因为该函数返回多个值而不得不定义很多没有的变量，Go语言可以借助多重返回值和匿名变量来避免这种写法，使代码看起来更优雅。

​	假如GetName()函数返回3个值，分别是firstName,lastName和nickName

```go
func GetName()(firstName,lastName,nickName string){
  return "li","mingqi","dove"
}
```

​	若指向获得nickName，则函数调用可以这样写

```go
_,_,nickName:=GetName()
```

这种写法可以让代码更清晰，从而大幅降低沟通的复杂度和维护的难度。

### 2、常量

​	1）基本常量

​	常量使用关键字const 定义，可以限定常量类型，但不是必须的，如果没有定义常量的类型，是无类型常量

​	2）预定义常量

​    Go语言预定义了这些常量 true、false和iota

​	iota比较特殊，可以被任务是一个可被编译器修改的常量，在每个const关键字出现时被重置为0，然后在下一个const出现之前每出现一个iota，其所代表的数字会自动加1.

```go
//预定义常量：true，false和iota
	// iota可以被认为是一个编译器修改的常量，在没有个const关键字出现时被重置为0，然后在下一个const出现之前，
	//没出现一次iota，其所在的数字会自动增1
	const (
		c0 = iota//c0=0
		c1 = iota//c1=1
		c2 = iota//c2=2
	)
	fmt.Println("c0:",c0)
	fmt.Println("c1:",c1)
	fmt.Println("c2",c2)
	const (
		x = 1 << iota // x==1(iota在每个const开头被重设为0)
		y = 1 << iota// y==2
		z = 1 << iota// z==4

	)
	fmt.Println("x:",x)
	fmt.Println("y:",y)
	fmt.Println("z:",z)
	const (
		r = iota*42
		t float32 =iota*42
		w =iota*42
	)
	fmt.Println("r:",r)
	fmt.Println("t:",t)
	fmt.Println("w:",w)
	const aa = iota
	const (
		bb = iota
	)
	fmt.Println("aa:",aa)
	fmt.Println("bb:",bb)
	const mark =2 << 3 //编译期运算的常量表达式
	fmt.Println("mark:",mark)
```

​	3)枚举

​	Go语言不支持emum关键字，而使用常量声明的方法定义枚举值：

```go
const(
  Sunday=iota
  Monday
  Tuesday
  Wendnesday
  Thursday
  Friday
  Saturday
  numberOfDays
)
```

### 3、类型

​	1）int 和int32在Go语言中被认为是两种不同类型的类型

​	2）Go语言定义了两个浮点型float32和float64，其中前者等价于C语言的float类型，后者等价于C语言的double类型

​	**3）go语言支持复数类型**

​		复数实际上是由两个实数（在计算机中使用浮点数表示）构成，一个表示实部（real）、一个表示虚部（imag）。也就是数学上的那个复数

​		复数的表示

```go
var value complex64//由两个float32 构成的复数
value1=3.2+12i
value2:=3.2+12i //value2是complex128类型
value3:=complex(3.2,12)//结果同value2
```

​    	实部与虚部

​		对于一个复数z=complex(x,y),就可以通过Go语言内置函数real(z)获得该复数的实部，也就是x，通过imag(z)获得该复数的虚部，也就是y

​	4）数组(值类型，长度在定义后无法再次修改，每次传递都将产生一个副本。)

   	声明数组可以使用[n]type方式，也可以使用make关键字

```go
[10] int
a:=make([10] int)
```

​	5）数组切片（slice）

​		数组切片（slice）弥补了数组的不足，其数据结构可以抽象为以下三个变量：

- 一个指向原生数组的指针

- 数组切片中的元素个数

- 数组切片已分配的存储空间。

​	6）Map
在go语言中Map不需要引入任何库，使用很方便

### 4、条件语句If

1. 条件语句不需要括号将条件包含起来
2. 无论语句体内有几条语句，花括号{}必须存在
3. 左花括号{必须与if或者else处于同一行
4. 在if之后条件语句之前，可以添加变量初始化语句，使用分号间隔
5. 在有返回值的函数中，不允许将最终的return语句包含在if...else结构中，否则会编译失败。

### 5、选择语句（switch）

1. ​	左花括号{必须和switch处于同一行
2. 条件表达式不限制常量或者证书
3. 单个case中，可以出现多个结果选项
4. Go语言中不需要break来明确退出一个case
5. 只有在case中明确添加fallthrough关键字，才会继续执行紧跟的下一个case
6. 可以不设定switch之后的表达式，在此种情况下，整个switch结构和if...else的逻辑作用一样

### 6、循环语句for

Go循环语句只支持for关键字，不支持while和do-while

1. 左{花括号必须与for处于同一行

2. Go语言不支持以逗号为间隔的多个赋值语句，必须使用平行的赋值语句来初始化多个变量

3. Go语言的for循环同样也支持continue和break来控制循环，但是它提供了一个更高级别的break，可以选择中断哪一个循环

   ```go
   for j:=0;j<5;j++{
     for i:=0;i<10 i++{
       if i>5{
         break JLoop //break语句终止的是JLoop标签处的外层循环
       }
       fmt.Println(i)
     }
   }
   JLoop:
   // ...
   ```



### 7、跳转语句

​	goto语句的语义非常简单，就是跳转到本函数内的某个标签

```go
fun myfunc(){
	i:=0
	HERE:
	fmt.Println(i)
	i++
	if i<10{
		goto HERE
	}
}
```
