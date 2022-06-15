---
title: "go语言顺序编程"
subtitle: ""
date: 2022-05-25T15:58:08+08:00
lastmod: 2022-05-25T15:58:08+08:00
draft: false
toc:
enable: true
weight: false
categories: ["程序设计"]
tags: ["go"]
---
# 类型

#### 布尔类型

#### 整型

#### 浮点型

#### 复数类型

#### 字符串

#### 字符类型

#### 数组



#### 数组切片

​	数组的长度在定义后无法再次修改；数组是值类型，每次传递都将产生一个副本

​	数组切片（slice）弥补了数组的不足，其数据结构可以抽象为以下三个变量：

- 一个指向原生数组的指针

- 数组切片中的元素个数

- 数组切片已分配的存储空间。

  ##### 1.创建数组切片

  ​	1）基于数组

  ```go
  	// 基于数组的创建切片
  	var myArry[10] int=[10]int {1,2,4,3,5,6,7,8,9,10}
  	var myslice [] int=myArry[:5]
  	fmt.Println("数组元素：")
  	for _,v:=range myArry{
  		fmt.Print(v," ")
  	}
  	fmt.Println()
  	fmt.Println("切片元素：")
  	for _,v:=range myslice{
  		fmt.Print(v," ")
  	}
  	//基于数组所有元素创建数组切片
  	myslice=myArry[:]
  	//基于前五个元素创建数组切片
  	myslice=myArry[:5]
  	//基于后五个元素创建数组切片
  	myslice=myArry[5:]
  ```

  ​    2）直接创建

  ```go
  //直接创建  创建一个初始元素个数为5的，元素初始值为0，
  	myslice1:=make([]int,5)
  	for i:=0;i<len(myslice1);i++{
  		fmt.Println("myslice1[",i,"]=",myslice1[i])
  	}
  	fmt.Println()
  	//创建一个初始元素个数为5的，元素初始值为0，并预留10个元素的存储空间
  	myslice2:=make([]int,5,10)
  	for i:=0;i<len(myslice2);i++{
  		fmt.Println("myslice1[",i,"]=",myslice2[i])
  	}
  	//直接创建并初始化包含5个元素的数组切片
  	myslice3:=[]int{1,2,3,44,5}
  	for i:=0;i<len(myslice3);i++{
  		fmt.Println("myslice1[",i,"]=",myslice3[i])
  	}
  	fmt.Println()
  ```

  ##### 2、元素遍历

  ​	操作数组元素的所有方法都适用于数组切片

  传统的元素遍历方法：

  ```go
  	for i:=0;i<len(myslice3);i++{
  		fmt.Println("myslice1[",i,"]=",myslice3[i])
  	}
  ```

  使用range关键字：range表达式有两个返回值，第一个是索引，第二个是元素的值

  ```go
  	for _,v:=range myslice{
  		fmt.Print(v," ")
  	}
  ```

  ##### 3、动态增减元素

  ​	数组切片支持go语言内置的cap()函数和len()函数。前者返回的事数组切片分配的空间大小，后者返回的是数组切片中当前锁存储的元素个数。

  ```go
  fun main(){
    mySlice:=make([]int,5,10)
    fmt.println("len(mySlice):",len(mySlice))
    fmt.println("cap(mySlice):",cap(mySlice))
  }
  ```

  输出的结果为：

  ![image-20220419171223803](https://limingqi.oss-cn-beijing.aliyuncs.com/mingqi_blog/img/go/image/img.png)

  如果向上例中的mySlice已包含的五个元素后面继续新增元素，可以使用append()函数。

  ```go
  mySlice=append(mySlice,1,2,3)
  ```

  ​	函数append的第二个参数其实是一个不定参数，我们可以按需求添加若干个元素，甚至直接将一个数组切片追加到另一个数组切片的末尾：

  ```go
  mySlice2:=[]int(8,9,10)
  mySlice=append(mySlice,mySlice2...)
  ```

  ​	需要注意的是，第二个参数后面加了三个点，即一个省略号，如果没有这个省略号的话会有编译错误，因为按照append()的语义，从第二个参数起，所有参数都是待附加的元素。因为mySlice中的元素类型为int，所以直接传递mySlice2是行不通的，加上省略号相当于把mySlice包含的所有元素打散后传入。

  ##### 4、基于数组切片创建数组切片

  ```go
  oldSlice:=[]int{1,2,3,4,5}
  newSlice:=oldSlice[:3]
  ```



​	有意思的是，选择的oldSlice元素甚至可以超过所包含的元素个数，比如newSlice可以基于oldSlice的前6个元素创建，虽然oldSlice只包含5个元素。只要这个选择的范围不超过oldSlice存储能力（即cap()返回的值），那么这个创建程序就是合法的。newSlice中超出oldSlice元素的部分都会被填上0。

##### 5、内容复制

​	数组切片支持go语言的另一种内置函数，copy(),用于将内容从一个数组切片复制到另一个数组切片。如果加入的两个数组切片不一样大，就会按照较小的那个数组切片的元素个数进行复制。

  ```go
  //内容复制
  s1:=[]int{1,2,3,4,5}
  s2:=[]int{6,7,8}
  copy(s1,s2)//只会复制s2的前三个元素到s1的前三个位置
  fmt.Println("result",s1)//result [6 7 8 4 5]
  
  ```

  ```go
  //内容复制
  s1:=[]int{1,2,3,4,5}
  s2:=[]int{6,7,8}
  copy(s2,s1)//只会复制s1的前三个元素到s2中
  fmt.Println("result",s2)//result [1 2 3]
  ```

#### map

​	map是一堆键值对的未排序集合，比如以身份证号作为唯一键来标识一个人的信息

```go
import "fmt"

type PersonInfo struct {
	ID string
	Name string
	Address string
}
func mapdemo()  {
	var personDB map[string] PersonInfo
	personDB=make(map[string] PersonInfo)
	// 往map里插入两条数据
	personDB["12345"]=PersonInfo{"12345","Tom","Room 203..."}
	personDB["1"]=PersonInfo{"1","Tom","Room 203..."}
  //从map中取数据。
	person,ok:=personDB["1234"]
	if ok {
		fmt.Println("Found person:",person.Name,"with id:",person.ID)
	} else {
		fmt.Println("Did not find person with 1234")
	}
  
}
```

##### 1、变量声明

​	变量声明基本上没有多余的元素，比如：

```go
var myMap map[string] PersonInfo
```

​	其中，myMap是声明的map变量，string是键的数据类型，PersonInfo是其中存放的值类型。

##### 2、创建

​	1）go语言内置函数make()创建一个新map

```go
myMap=make(map[string] PersonInfo)
```

​	2）创建时指定该map的存储能力：

```go
personDB=make(map[string] PersonInfo,100)
```

3. 创建并初始化map

   ```go
   personDB=make(map[string] PersonInfo{"1","mingqi","address"})
   ```

##### 3、元素赋值

  	赋值的过程简单明了，就是将键和值用下面的方式对应起来

```go
myMap["1"]=PersonInfo{"1","Tom","Room 203..."}
```

##### 4、元素删除

​	go语言提供了一个内置函数delete(),用于删除容器内的元素

```go
delete(myMap,"1234")
```

​	上面的代码将myMap中删除键为"1234"的键值对。如果不存在，这个调用将什么都不发生，也不会有什么副作用，但是如果传入map变量的值是nil，则抛出异常（panic）

##### 5、元素查找

​	在Go语言中，map的查找功能设计的比较精巧，而在其他语言中，我么要判断能否获取到一个值并不是意见容易的事，判断能否从map中获取到一个值的常规做法是：

1)声明并初始化一个变量为空；

2）试图从map中获取相应的值到该变量中

3）判断该变量是否依旧为空，如果为空则表示map中没有包含该变量。

​	这种用法比较啰嗦，而且判断变量是否为空这句并不能真正表意（是否成功取到对应的值），从而影响代码的可读性和可维护性，有些库甚至会设计一个键不存在而抛出的异常，让开发者用起来胆战心惊，不得不一层层嵌套try-catch语句，这更是不人性化的设计，在go语言中：

```go
  //从map中取数据。
	person,ok:=personDB["1234"]
	if ok {
		fmt.Println("Found 		person:",person.Name,"with id:",person.ID)
	} else {
		fmt.Println("Did not find person with 1234")
	}
```

# 流程控制

#### 条件语句

​	关于条件语句的样例代码如下：

```go
if a<5 {
	return 0
}else{
  return 1
}
```

​	需要注意一下几点：

​	1、条件语句不需要用括号将条件含起来；

​	2、无论语句体内有几条语句花括号{}都必须存在的

​	**3、左花括号{必须于if或者else处于同一行；**

​	4、在If之后，条件语句之前，可以添加变量初始化语句，使用;分号间隔。

​	5、在有的返回值的函数中，不允许将最终的return语句包含在if...else...结构中，否则编译会失败。

#### 选择语句

​	根据传入的条件不同，选择语句会选择执行不同的语句

```go
switch i{
  case 0：
 	 fmt.Printf("0")
  case 1：
 	 fmt.Printf("1")
  case 2：
 	 fmt.Printf("2")  
  case 3：
 	 fmt.Printf("3")
  default：
  fmt.Printf("Default")
}
```

有意思的事，switch后面的表达式甚至不是必须的

```go
switch{
	case 0<=num&& num<=3
		fmt.Printf("0-3")
  case 4<=num&& num<=7
		fmt.Printf("4-7")
 	case 8<=num&& num<=9
		fmt.Printf("8-9")
}
```

#### 循环语句

​	go语言的循环语句只支持for关键字，不支持while和do-while结构

```go
sum:=0
for i:=0;i<10;i++{
  sum+=i
}
```

​	for后面的条件表达式不需要用()包含起来，考虑无限循环的场景：

```go
sum:=0
for{
	sum++
	if sum>100{
		break
	}
}
```

​		在表达语句中也支持多重赋值：

```go
a:=[]int{1,2,3,4,5,6}
for i,j:=0,len(a)-1;i<j;i,j=i+1,j-1{
	a[i],a[j]=a[j],a[i]
}
```

​	使用循环语句时候，需要注意一下几点：

​	1）左花括号{必须和for处于同一行

​	2）go语言不支持以逗号为间隔的多个赋值语句，必须使用平行赋值的方式来初始化多个变量

​	3）go语言的for循环同样支持continue和break来控制循环，但它提供了一个**更高级的break，可以选择中断哪一个循环：**

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

#### 跳转语句

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

# 函数

​	在go语言中，函数的基本组成为：关键字func、函数名、参数列表、函数体和返回语句。

#### 函数定义

```go
package mymath
import "errors"

func Add(a int,b int)(ret int,err error){
  if a<0||b<0{
    err=errors.New("不能为负数")
    return
  }
  return a+b,nil//支持多重赋值
}
```

​		如果参数列表中若干个相邻参数类型相同，比如上面的例子中的a，b，则可以在参数列表中省略前面变量的类型声明：

```go
fun Add（a,b int）(ret int,err error){
// todo
}
```

​	如果返回值列表中返回多个返回值的类型相同，也可以用同样的方法合并

```go
fun Add（a,b int）int{
// todo
}
```

#### 函数调用

​	实现导入该函数所在的包，就可以直接按照如下所示的方式调用函数：

```go
import "mymath"
c:=mymath.Add(1,2)
```

​	在go语言中函数支持多重返回值，在之后的内容中会介绍。利用函数的多重返回值和错误处理机制，我们可以很容易写出优雅美观的代码。

​	go语言中函数名字的大小写不仅仅是风格，更直接体现了该函数的可见性，需要牢记这样的规则，**小写字母开头的函数只在本包内可见，大写字母开头的函数才能被其他包使用，这个规则也适用于类型和变量的可见性**

#### 不定参数

##### 	1、不定参数类型

​		不定参数是指函数传入的参数个数为不定数量。为了做到这一点，首先要将函数定义为接受不定参数类型：

```go
//接受不定数量的参数，这些参数类型全部是int
func myfunc(args ...int){
  for _,arg:=range args{
    fmt.Println(arg)
  }
}
//调用
myfunc(1,2,4)
myfunc(1,2)
```

​	形如...type格式的类型只能作为函数的参数类型存在，并且只能是最后一个参数。他是一个语法糖，即这种语法对语言的功能并没有影响，但是更方便程序员使用。通常来说，使用语法糖能够增加程序的可读性，从而减少程序出错的机会。

​	从内部实现机理上来说，类型...type本质上是一个数组切片，也就是[]type,这也就是为什么上面的参数args可以用for循环来获得每个传入的参数。

​	假如没有...type这样的语法糖，开发者将不得不这么写：

```go
func myfunc2(args []int){
  for _,arg:=range args{
    fmt.Println(arg)
  }
}
```

​	从函数的实现的角度来看，这没有任何影响，该怎么写就怎么写，但从调用方来说，情形则完全不同：

```go
myfunc2([]int{1,2,4})
```

##### 	2、不定参数的传递

​		假设有另一个变参函数叫myfunc3(args ...int),下面的代码演示了如何向其传递变参

```go
func myfunc(args ...int){
  myfunc3(args...)//按原样传
  //传递片段，实际上任一的int slice都可以传进去
  myfunc3(args[1:]...)
}
```

##### 	3、任意类型的不定参数

​		之前的例子将不定参数类型约束为int，如果你希望传递任意类型，可以指定类型为interface{}

```go
package main
import "fmt"
func MyPrintf(args ...interface{}){
  for _,arg:=range args{
    switch arg.(type){
      case int:
     	 	fmt.Println(agr,"是int 类型")
      case string:
     			 fmt.Println(agr,"是string类型")
      case int64:
     	 		 fmt.Println(agr,"是int64 类型")
      default:
     			 fmt.Println(agr,"error")
    }
  }
}
func main(){
  var v1=1
  var v2 int64=123
  var v3 string="abc"
  MyPrintf(v1,v2,v3)
}
```

#### 多返回值

```go
func (file *File) Read(b []byte)(n int,err Error)
```

上面的方法原型可以看出，可以给返回值命名，就像函数的输入参数一样。函数值被命名之后，他的值在函数开始的时候被自动初始化为空。在函数中执行不带任何的return语句时，会返回对应的返回值变量的值。

​	go语言并不需要强制命名返回值，但是命名后可以使代码更清晰，可读性更强，同时也可以用于文档

​		如果调用方调用了一个具有多个返回值的方法，却不想关心其中的某个返回值，可以简单的使用下划线_来跳过这个返回值

```go
n:_=f.Read(buf)
```

#### 匿名函数与闭包

1、匿名函数

​	在go语言里，函数可以像普通变量一样被传递或使用，支持随时在代码里定义匿名函数

​	匿名函数由一个不带函数名的函数声明和函数体组成：

```go
func (a,b int,z float)bool{
  return a*b<int(z)
}
```

​	匿名函数可以直赋值给一个变量或者直接执行

```go
f:=func(x,y int)int{
  return x+y
}
func(ch chan int){
  ch<-ACK
}(reply_chan)//花括号后直接跟参数列表表示函数调用
```

2、闭包

​				go的匿名函数是一个闭包

​	**基本概念**

​			闭包是可以包含自由(未绑定的特定资源)变量的代码块，这些变量不在这个代码块内或者任何全局上下文中定义，而是在定义代码块的环境中定义。要执行的代码块（由于自由变量包含在代码块中，所以这些自由变量以及他们引用的对象没有被释放。）为自由变量提供绑定的计算环境（作用域）

​	**闭包的价值**

​		闭包的价值在于可以作为函数对象或者匿名函数，对于类型系统而言，这意味着不仅要表示数据还要表示代码。支持闭包的多语言都将函数作为第一级对象，就是说这些函数可以存储到变量中作为参数传递给其他函数，最重要的事能够被函数动态创建和返回。

​	**Go语言中的闭包**

​		go语言中的闭包同样也会引用到函数外的变量，闭包的实现确保只要闭包还被使用，那么被闭包引用的变量就会一直存在：

```go
func main()  {
	var j int =5
	a:= func()(func()) {
		var i int =10
		return func() {
			fmt.Println("i,j:%d,%d\n",i,j)
		}
		
	}()
	a()
	j*=2
	a()
	}
```

上述例子的执行结果是：

![001](https://limingqi.oss-cn-beijing.aliyuncs.com/mingqi_blog/img/go/image/001.png)

​	在上面的例子中，变量a指向的闭包函数引用了局部变量i和j，i的值被隔离,在闭包外不能被修改，改变j的值后再次调用a，发现结果是修改过的值。

​	在变量a指向的闭包函数中，只有内部的匿名函数才能访问变量i，而无法通过其他途径访问到，因此保证了i的安全性

# 错误处理

#### error接口

​	Go语言引入了关于错误处理的标准模式，即error接口，该接口的定义如下：

```go
type error interface{
  Error() string
}
```

​	对于大多数函数，如果需要返回错误，大致上都可以定义为如下模式，将error作为返回多种返回值中的最后一个，但这并非是强制要求的：

```go
fun Foo（parm int）(n int err error){
//todo something
}
//调用的代码建议按如下方式处理错误情况
n,err:=Foo(0)
if err!=nil{
  //处理错误
}else{
  //返回n
}
```

自定义库的实现：

```go
type PathError struct{
  Op string
  Path string
  Err error
}
//实现了Error方法
func (e *PathError) Error()string{
  return e.Op+" "+e.Path+":"+e.Err.Error()
}
// 下面代码执行syscall.Stat()失败时，将该err包装到一个PathError对象中返回
func Stat(name string)(fi FileInfo,err error){
  var stat syscall.Stat_t
  err=syscall.Stat(name,&stat)
  if err!=nil{
    return nil,&PathError("stat",name.err)
  }
  return fileInfoFromStat(&stat,name),nil
}
//如果在处理错误时获取详细信息，而不仅仅满足打印一句错误信息，那么就需要用到类型转换的知识了 
fi,err:=os.Stat("a.txt")
if err!=nil{
  if e,ok:=err.(*os.PathError);ok&&e.Err!=nil{
    //获取PathError类型变量e中的其他信息并处理
  }
}
```

#### defer

defer 在函数执行最后处理一些收尾工作，一个函数中可以有多个defer语句，遵照先进后出的原则，即最后一个defer语句将最先被执行。不过当你需要为defer语句到底哪个先执行这种细节而烦恼的时候，说明你的代码架构需要调整一下了。

#### panic()和recover()

panic():

​	当一个函数执行过程中调用一个panic()函数时，正常的函数执行流程将被立即终止，但函数中之前使用defer关键字延迟执行的语句将正常开展执行，之后该函数将返回到调用函数，并导致逐层向上执行panic流程，直至所属的goroutine中所有正在执行的函数被终止。错误信息将被报告，包括在调用panic函数时传入的参数，这个过程称为错误处理流程

recover()

​	用于终止错误处理流程，一般情况下，recover()应该在一个使用defer关键字的函数中执行以有效截取错误处理流程。如果没有发生异常的goroutine中明确调用恢复过程（使用recover关键字），会导致该goroutine所属的进程打印异常信息后直接退出。

以下为一个常见的场景：

​	我们对于foo()函数的执行要么心里没底感觉可能会触发错误处理，或者自己在其中明确加入了按特定条件触发错误处理的语句，那么可以使用如下方式在调用代码中截取recover():

```go
defer func(){
  if r:=recover();r!=nil{
    log.Printf("运行错误",r)
  }
}()
foo()
```

​	无论fool()中是否触发了错误处理流程，该匿名defer函数都将在函数退出时执行，假如foo()中触发了错误处理流程，recover函数执行将是的该错误处理过程终止。如果错误处理流程触发时，程序传给panic函数的参数不为nil，则该函数还会打印出详细的错误信息。
