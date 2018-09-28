---
title: Go语言类型系统
date: 2018-09-25 20:07:45
tags: Golang
---
* 任何时候，创建一个变量并初始化为其零值，习惯是使用关键字 var。这种用法是为了更明确地表示一个变量被设置为零值。如果变量被初始化为某个非零值，就配合结构字面量和短变量声明操作符来创建变量。
* 使用结构字面量生命一个结构类型的变量
```go
type user struct {
	name		string
	email		string
	ext			int
	privileged bool
}

lisa := user {
	name:		"Lisa",
	email:		"lisa@email.com",
	ext:			123,
	privileged: true,    //最后一个也要有逗号
}

james := user{"James", "james@email.com", 123, true}  //每个值也可以分别占一行，不过习惯上这种形式会写在一行里，结尾不需要逗号。必须和结构声明中字段的顺序一致
```
* 扩展新的结构类型
```go
type admin struct {
	person user
	level string
}

//创建变量
fred := admin {
	person: user{
		name:		"Lisa",
		email:		"lisa@email.com",
		ext:			123,
		privileged:	true,
	},
	level: "super",
}
```
* 可以基于已有类型来声明新的类型，但这两个类型是不同的类型
```go
//编译错误
package main

type Duration int64

func main()  {
	var dur Duration
	dur = int64(1000)
}
```
* 方法可以给用户定义的类型添加新行为，实际也是函数，只是在声明时，关键字func和方法名之间增加了一个参数
    * 值接收者，使用值接收者声明方法，调用时会使用这个值的副本来执行
    * 指针接收者，使用指针接收者声明的方法，调用时会使用这个值的来执行
不管值还是指针都可以调用上述两种方法，只是在调用的时候才会做副本拷贝或直接访问指针
* 引用类型
切片、映射、通道、接口、函数。当声明这些变量时，创建的变量被称作标头值，是为复制设计的，方便共享底层数据
* 方法集
方法集定义了一组关联到给定类型的值或者指针的方法。定义方法时使用的接收者的类型决定了这个方法是关联到值，还是关联到指针，还是两个都关联。
从接收者类型的角度来看方法集

| Methods Receivers | Values |
|------------------- | ------ |
| (t T) | T and *T |
| (t *T) | *T |

这个规则说，如果使用指针接收者来实现一个接口，那么只有指向那个类型的指针才能够实现对应的接口。如果使用值接收者来实现一个接口，那么那个类型的值和指针都能够实现对应的接口。
为什么会有这种限制？事实上，编译器并不是总能自动获得一个值的地址
* 嵌入类型  --- 和继承类似
嵌入类型是将已有的类型直接声明在新的结构类型里。被嵌入的类型被称为新的外部类型的内部类型。
通过嵌入类型，与内部类型相关的标识符会提升到外部类型上。这些被提升的标识符就像直接声明在外部类型里的标识符一样，也是外部类型的一部分。这样外部类型就组合了内部类型包含的所有属性，并且可以添加新的字段和方法。外部类型也可以通过声明与内部类型标识符同名的标识符来覆盖内部标识符的字段或者方法。这就是扩展或者修改已有类型的方法。

```go
package main

import "fmt"

type notifier interface {
	notify()
}

type user struct {
	name string
	email string
}

func (u *user) notify()  {
	fmt.Printf("Sending user email to %s<%s>\n", u.name, u.email)
}

type admin struct {
	user
	level string
}

func (a *admin) notify()  {
	fmt.Printf("Sending admin email to %s<%s>\n", a.name, a.email)
}

func main()  {
	ad := admin{
		user{
			name: "john smith",
			email: "john@yahoo.com",
		},
		"super",
	}

	sendNotification(&ad)

	ad.user.notify()

	ad.notify()
}

func sendNotification(n notifier)  {
	n.notify()
}
```
