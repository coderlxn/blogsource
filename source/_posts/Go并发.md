---
title: Go并发
date: 2018-09-27 21:13:59
tags: Golang
---
#### 竞态问题的解决方案
* 原子函数
原子函数可以保证某个操作的原子性，避免产生竞态
```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
	"time"
)

var (
	shutdown int64
	wg1 sync.WaitGroup
)

func main()  {
	wg1.Add(2)

	go doWork("A")
	go doWork("B")

	time.Sleep(1 * time.Second)

	fmt.Println("Shutdown now")
	atomic.StoreInt64(&shutdown, 1)

	wg1.Wait()
}

func doWork(name string)  {
	defer wg1.Done()

	for {
		fmt.Printf("Doing %s Work\n", name)
		time.Sleep(250 * time.Millisecond)

		if atomic.LoadInt64(&shutdown) == 1 {
			fmt.Printf("Shutting %s Down\n", name)
			break
		}
	}
}

```
* 互斥锁
互斥锁用于在代码上创建一个临界区，保证同一时间只有一个 goroutine 可以执行这个临界区代码。
```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

var (
	mutexCounter int
	mutexWg sync.WaitGroup
	mutex sync.Mutex
)

func main()  {
	mutexWg.Add(2)

	go mutexIncCounter(1)
	go mutexIncCounter(2)

	mutexWg.Wait()
	fmt.Println("Final Counter:", mutexCounter)
}

func mutexIncCounter(id int)  {
	defer mutexWg.Done()

	for count := 0; count < 2; count++ {

		mutex.Lock()
		{
			value := mutexCounter
			runtime.Gosched()

			value++
			mutexCounter = value
		}
		mutex.Unlock()
	}
}
```

#### 通道
1. 无缓冲的通道
	* 在接收前没有任何能力保存任何值的通道，要求发送goroutine和接收goroutine同时准备好。
	```go
	package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

var wgNoCache sync.WaitGroup

func init() {
	rand.Seed(time.Now().UnixNano())
}

func main() {
	court := make(chan int)

	wgNoCache.Add(2)

	//两个选手
	go player("Nadal", court)
	go player("Djokovic", court)

	//发球
	court <- 1

	wgNoCache.Wait()
}

func player(name string, court chan int)  {
	//
	defer wgNoCache.Done()

	for {
		//等待球被击打过来
		ball, ok := <-court
		if !ok {
			fmt.Printf("Player %s Won\n", name)
			return
		}

		n := rand.Intn(100)
		if n%13 == 0 {
			fmt.Printf("Player %s Missed\n", name)

			//关闭通道，我们输了
			close(court)
			return
		}

		//显示击球数
		fmt.Printf("Player %s Hit %d\n", name, ball)
		ball++

		//将球打向对手
		court <- ball
	}
}
	```
2. 有缓冲的通道