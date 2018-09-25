---
title: Just Go
date: 2018-09-19 19:23:20
tags: Golang
---
##### 在语言层面上提供并发和传值能力
```go
package main

import "fmt"

func sum(a []int, result chan int)  {
	sum := 0
	for _, v := range a {
		sum += v
	}
	result <- sum
}

func main()  {
	a := []int{2, 3, 5, 6, 10, -5, 1, 0}
	result := make(chan int)
	go sum(a[:len(a)/2], result)
	go sum(a[len(a)/2:], result)
	x, y := <-result, <-result

	fmt.Println(x, y, x+y)
}
```
##### 使用组合而不是继承来扩展功能
```go
package main

import "fmt"

type Phone interface {
	call()
}

type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call()  {
	fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
	fmt.Println("I am iPhone, I can call you!")
}

func main()  {
	var phone Phone

	phone = new(NokiaPhone)
	phone.call()

	phone = new(IPhone)
	phone.call()
}
```
##### 导入包的时候，会默认在main方法前调用init方法。可以保证某些功能得到初始化
```go
package main
import {
	"database/sql"
	_ "github.com/goinaction/code/chapter3/dbdriver/postgres"
}

func main() {
	sql.Open("postgres", "mydb")   //之所以能够使用，是因为postgres中通过init将自身注册到了sql包
}
```
##### 数组字面量
数组字面量允许声明数组里元素数量同时指定每个元素的值
```go
array := [5]int{10, 20, 30, 40, 50}
// 容量由初始化值的数量决定
array := [...]int{10, 20, 30, 40, 50}
//只初始化部分元素
array := [4][2]int{1: {0: 20}, 3: {1: 41}}
```
##### 切片长度和容量的区别
长度是当前可以访问的元素个数，容量是切片底层数组的长度
```
//空切片
var slice []int
//或者
slice := make([]int, 0)
```
![inC6Mt.png](https://s1.ax1x.com/2018/09/20/inC6Mt.png)
对底层数组容量是k的切片slice[i:j]来说
长度 = j - i
容量 = k - i
对于slice[i:j:k]来说
长度 = j - i
容量 = k - i
##### 切片增长
要使用 append，需要一个被操作的切片和一个要追加的值，返回一个包含修改结果的新切片。append总会增加新切片的长度，而容量可能会不变。
```go 
slice := []int{10, 20, 30, 40, 50}
//创建一个长度2，容量4的新切片
newSlice := slice[1:3]
//分配一个新元素，元素值为60
newSlice = append(newSlice, 60)
```
![inPskF.md.png](https://s1.ax1x.com/2018/09/20/inPskF.md.png)
