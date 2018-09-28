---
title: Go问题收集
date: 2018-09-26 19:50:08
tags: Golang
---

#### 引入当前目录下的其他包
如果当前目录不在GoPath中，要引入当前目录的其他包时需要使用相对路径
```go
import (
    "../controllers" //这里就是导入上一级目录中的controllers
    "./models" //同一级目录中的models
    "./models/entitys" //当前目录下的entitys
    "../../routers" //上两级目录的routers
)
```

#### 使用routine时，需要注意主线程不能退出
可以使用sync包中的WaitGroup来阻止线程退出
```go
package main

import (
	"fmt"
	"sync"
)

func main()  {
	var waitGroup sync.WaitGroup
	waitGroup.Add(5)

	for  a := 0; a < 5; a++ {
		go func() {
			fmt.Println("run in function")
			waitGroup.Done()
		}()
	}

	waitGroup.Wait()
}
```