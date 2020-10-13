+++
title="GO浪知识点小记 | new 与 make"
tags=["golang-notes"]
categories=["GO浪知识点小记"]
date="2020-10-13T00:00:00+08:00"
toc=true
+++


<!--  -->
new 与 make
<!--more-->

## new

在官方文档中，new 函数的描述如下
```go
// The new built-in function allocates memory. The first argument is a type,
// not a value, and the value returned is a pointer to a newly
// allocated zero value of that type.
func new(Type) *Type
```

可以看到，new 只能传递一个参数，该参数为一个任意类型，可以是Go语言内建的类型，也可以是你自定义的类型

那么 new 函数到底做了哪些事呢：

+ 分配内存

+ 设置零值

+ 返回指针（重要）

举个例子

```go
import "fmt"

type Student struct {
   name string
   age int
}

func main() {
    // new 一个内建类型
    num := new(int)
    fmt.Println(*num) //打印零值：0

    // new 一个自定义类型
    s := new(Student)
    s.name = "wangbm"
}
```
## make

在官方文档中，make 函数的描述如下

```go
//The make built-in function allocates and initializes an object //of type slice, map, or chan (only). Like new, the first argument is // a type, not a value. Unlike new, make’s return type is the same as // the type of its argument, not a pointer to it.

func make(t Type, size …IntegerType) Type
```
注意，因为这三种类型是引用类型，所以必须得初始化（size和cap），但是不是置为零值，这个和new是不一样的。

```go
//切片
a := make([]int, 2, 10)

// 字典
b := make(map[string]int)

// 通道
c := make(chan int, 10)
```
## 总结
new：为所有的类型分配内存，并初始化为零值，返回指针。

make：只能为 slice，map，chan 分配内存，并初始化，返回的是类型。

另外，目前来看 new 函数并不常用，大家更喜欢使用短语句声明的方式。

```go
a := new(int)
*a = 1
// 等价于
a := 1
```

但是 make 就不一样了，它的地位无可替代，在使用slice、map以及channel的时候，还是要使用make进行初始化，然后才可以对他们进行操作。
原文: http://golang.iswbm.com/en/latest/c02/c02_03.html