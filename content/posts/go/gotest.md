---
title: "go语言面向对象编程"
date: 2022-05-24T18:38:36+08:00
draft: false
subtitle: ""
date: 2022-05-25T15:58:08+08:00
lastmod: 2022-05-25T15:58:08+08:00
toc:
enable: true
weight: false
categories: ["程序设计"]
tags: ["go"]
---

# 1、类型系统

类型系统是指一个语言的类型体系结构，一个典型的类型系统通常包含如下基本内容：

- 基础类型，如byte、int、bool、float等
- 复合类型：如数组、结构体、指针等
- 可以指向任意对象的类型
- 值语义和引用语义
- 面向对象
- 接口

## 为类型添加方法

```go
type Integer int
func(a Integer) Less(b Integer) bool{
  return a<b
}
```

​	只有在修改修改对象的时候才必须用指针，他不是go语言的约束，而是一种自然约束

```go
//这里为Integer类型增加了add方法，由于Add方法需要修改对象的值，所以需要用指针引用
func (a *Integer)Add(b Integer){
  *a+=b
}
//调用如下：
func main(){
  var a Integer=1
  var b Integer=1
  b.Add(2)
  fmt.Println("a=",a)//a=3
  fmt.Println("b=",b)//b=1
  //类型都是基于值传递的，要想修改变量的值，只能传递指针。
}
//如果实现方法时传入的不是指针而是值，那么运行程序得到的结果会是a=1
func (a Integer)Add1(b Integer){
  a+=b
}
```

​

## 值语义和引用语义

值语义：

​	基本类型

​	复合类型，如数组、结构体和指针等

引用语义：（看起来）

​	数组切片

​	map

​	chaannel

​	接口

## 结构体

​	go语言放弃了 包括集成在内的大量面向对象特性，只保留了组合这个最基础的特性

```go
type Rect struct{
  x,y float64
  width,height float64
}
//为矩形类增加方法来计算面积
func (r *Rect)Area()float64{
  return r.width*height
}
```

# 2、初始化

​

```go
r1:=new(Rect)
r2:=&Rect()
r3:=&Rect(0,0,100,200)
r4:=&Rect(width:100,height:200)//未显示初始化的变量都会被初始化为该类型的零值。

```

go语言没有构造函数的概念，对象的创建通常交由一个全局的创建函数来完成，以NewXXX来命名，表示构造函数:

```go
func NewRect(x,y,width,height float64)*Rect{
	return &Rect{x,y,width,height}
}
```

# 3、匿名组合

​	确切的说，Go语言也提供了继承，但是采用了组合的文法，所以我们称其为匿名组合

```go
type Base struct{
  Name string
}
func (base *Base)Foo(){
  //
}
func (base *Base)Bar(){
  //
}
type Foo struct{
  Base
  // *Base 以指针的方式从一个类型派生，只是创建实例的时候需要外部提供一个Base类示例的指针。
  //
}
//从Base类"继承"并改写了Bar方法
func (foo *Foo)Bar(){
  foo.Base.Bar()
}
//Job类匿名组合了一个log.Logger指针
type Job struct {
  Command string
  *log.Logger
}
//在合适的赋值后，我们在Job类型的所有成员方法中都可以很舒服地借用所有log.Logger提供的方法：
func (job *Job)Start(){
  job.Log("start now")
  //...
  //对于job的实现者来说，他甚至根本不用意识到log.Logger的存在，这就是匿名的魅力所在
  job.Log("end now")
}
```

​	不管是非匿名函数的类型组合还是匿名组合，被组合的类型所包含的方法虽然升级成了外部这个组合类型的方法，但其实他们被组合方法调用时接收者并没有改变，比如上面的这个Job的例子，即使组合后调用方式编程了job.Log(),但Log内部的接收者仍然是log.Logger指针，因此在Log中不能访问job的其他成员变量和方法。

# 4、可见性

​	定义类、变量、方法名称首字母大写即可对其他包可见



# 5、接口

​	**接口是go语言整个类型系统的基石，让其在基础编程哲学上的探索上达到前所未有的高度。go语言的编程哲学因为有接口而趋近完美。**

## 其他语言的接口

```java
interface IFoo{
  void Bar()
}
class Foo implements IFoo{
  //... java 文法
}
class Foo : public IFoo{
  //c++文法
}
IFoo* foo=new Foo;
```

​	即使另外一个接口IFoo2实现了与IFoo完全一样的接口方法，甚至名字也叫IFoo，只不过位于不同的命名空间下，编译器也会认为上面的类Foo只实现了IFoo接口而没有实现IFoo2接口。

​	这类接口我们称之为侵入式接口。主要表现在实现类需要明确自己实现了某个接口。这种强制性的接口集成是面向对象编程思想发展过程中一个遭受相当多质疑的特性。结下来我们来讨论下为什么是这个问题。

​	假如我们现在要实现一个简单的搜索引擎（SE），他需要依赖两个模块，一个是哈希表（HT），一个时HTML分析器（HtmlPaser）

​	搜索引擎的实现者认为，SE对HT的依赖是确定性的，所以不需要在SE和HT之间定义接口，而是直接通过import（或者include）的方式使用HT；而模块SE对HtmlParser的依赖是不确定的，未来可能需要由wordParser、pdfPaser等模块来替代HtmlParser，以达到不同业务要求，为此，他定义了SE和HtmlParser之间的接口，在模块Se中通过接口调用的方式引用模块HtmlParser。

​	应当注意到，接口的需求方式SE，只有SE才知道接口需要定义成什么样子，但是接口的实现方式HtmlParser。基于模块设计的单项依赖原则，HtmlParser实现自身业务的时候，不应该关心某个具体使用方的要求。在实现的时候，甚至还不知道未来有一天SE会用上他。

​	期望模块HtmlPaser能够知道需求方需要的所有接口，并提前声明实现这些接口是不合理的，同样的道理发生在SE自己身上，SE并不能够预计未来有哪些需求方会用到自己，并实现他们的所要求的接口。

​	这个问题在设计标准库的时候变得更加突出，比如我们实现的File类，他有下面这些方法：

```go
Read(buf []byte)(n int,err error)
Write(buf []byte)(n int,err error)
Seek(off int64 whence int)(pos int64,err error)
Close()error
```

​	那么到底是应该定义一个IFile接口，还是应该定义一系列的IRead，IWriter、ISeeker、ICloser接口，让File从他们集成好呢，脱离了实际用户的场景，讨论这两个设计哪个更好并无意义，问题在于，实现File类的时候，我怎么知道外部会怎么使用它呢。正式因为这中不合理的设计，实现JAVA、C#类库中的每个类都需要纠结以下两个问题：

1、我提供哪个接口好呢

2、如果两个类实现了相同的接口，应该把接口放到哪个包好呢？

## 非侵入式接口

​	在go语言中，一个类只要实现了接口要求的所有函数，我们就说这个类实现了该接口：

```go
package main
import "fmt"
/**
定义File类，并实现有Read，Write，Seek，Close等方法
 */
type  File struct {
	//...
}
func (f *File) Read(buf []byte)(n int,err error){
	return 0, err
}
func (f *File) Write(buf []byte)(n int,err error){
	return 0, err
}
func (f *File) Seek(off int64,whence int)(pos int,err error){
	return 0, err
}
func (f *File) Close() error{
	return nil
}
//假如有以下接口

type IFile interface {
	Read(buf []byte)(n int,err error)
	Write(buf []byte)(n int,err error)
	Seek(off int64,whence int)(pos int,err error)
	Close() error
}
type IReader interface {
	Read(buf []byte)(n int,err error)
}
type Iwriter interface {
	Write(buf []byte)(n int,err error)
}
type ISeeker interface {
	Seek(off int64,whence int)(pos int,err error)
}
type ICloser interface {
	Close() error
}
// 尽管File类并没有从这些接口继承，甚至不知道这些接口的存在，但是File类实现了这些接口可以进行赋值
func demo()  {
	var f1 IFile=new(File)
	var f2 IReader=new(File)
	var f3 Iwriter=new(File)
	var f4 ICloser=new(File)
	fmt.Println(f1,f2,f3,f4)
}
```

​	go语言的非侵入式接口，看似只是做了很小的文法调整，实则影响深远

​	其一，Go语言的标准库，再也不需要回执类库的继承图，在go语言中，类的继承树并无意义，你知道知道这个类实现了那些方法，每个方法是啥含义就足够了。

​	其二，实现类的时候只需要关心自己应该提供那些方法，不需要在纠结接口需要拆的多细才合理，接口由使用方按需定义，而不用事前规划。

​	其三，不用为了实现一个接口而导入一个包，因为多引用一个外部包，就意味着更多的耦合。接口由使用方按自身需求定义，使用方无需关系是否有其他模块定义过类似的接口。

## 接口赋值

​	将对象实例赋值给接口

​		要求该对象实现了接口要求的所有方法

```go
type Integer int
func(a Integer)Less(b Integer)bool{
  return a<b
}
func(a *Integer)Add(b Integer){
  *a+=b
}
//我们定义接口LessAdder
type LessAdder interface{
  Less(b Integer) bool
  Add(b Integer)
}
//赋值 
var a Integer =2
var b LessAdder=&a//正确    （1）
var b LessAdder=a //错误    （2）
// （1）正确的原因在于，Go语言可以根据下面的函数
func (a Integer)Less(b Integer)bool
//生成一个新的Less方法
func (a *Integer)Less(b Integer)bool{
  return (*a).Less(b)
}
//这样类型*Integer就既存在了Less方法，也存在了Add方法，满足LessAdder接口，而从另一个方面来说，根据
func(a *Integer)Add(b Integer)
//这个函数无法自动生成以下这个成员方法
func(a *Integer)Add(b Integer){
  (&a).Add(b)
}
//因为(&a).Add(b)改变的知识参数a，对外部实际要操作的对象没有影响，这不符合用户的预期，所以，Go语言不会自动为其生成该函数，因此类型Integer只存在Less方法，缺少Add方法，不满足LessAdder接口，所以（2）不能赋值

```

​	将一个接口赋值给另一个接口

​			只要两个接口拥有相同的方法列表（次序不同不要紧），那么他们就是等同的，可以相互赋值。接口赋值不要求两个接口不许等价。如果A接口的方法列表是接口B的方法列表的子集，那么接口B可以赋值给接口A

## 接口查询

## 类型查询

```go
var v1 interface{}=...
switch v:v1.(type){
  case int://现在v的类型时int
  case string://v的类型时string
  ...
}
```

使用反射也可以进行类型查询。

## 接口组合

```go
//ReadWriter接口将基本的Read和Write方法组合起来
type ReadWriter interface{
  Reader
  Writer
}
//这个接口组合了Reader和Writer两个接口，等同于如下写法：
type ReadWriter interface{
  Read(p []byte)(n int,err error)
  Write(p []byte)(n int,err error)
}
//这两种写法的表意完全相同：ReadWriter接口既能做Reader接口的所有事情，又能做Writer接口的所有事情
```

可以认为接口组合是类型匿名组合的一个特定场景，只不过过接口只包含方法，不包含任何成员变量。

## Any类型

由于Go语言中任何对象实例都满足空接口interface{},所以interface{}看起来像是可以指向任何对象的Any类型。