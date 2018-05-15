title: Go的select-case学习
date: 2018/05/15 16:11:25
categories:
- Go
tags: [Go]
---

golang 的 select 就是监听 IO 操作，当 IO 操作发生时，触发相应的动作。

在执行select语句的时候，运行时系统会`自上而下地判断每个case中的发送或接收操作是否可以被立即执行`(立即执行：意思是当前Goroutine不会因此操作而被阻塞)

select的用法与switch非常类似，由select开始一个新的选择块，每个选择条件由case语句来描述。与switch语句可以选择任何可使用相等比较的条件相比，select有比较多的限制，其中最大的一条限制就是每个case语句里必须是一个IO操作，确切的说，应该是一个面向channel的IO操作。

下面这段话来自官方文档：

"select" statement chooses which of a set of possible send or receive operations will proceed. It looks similar to a "switch" statement but with the cases all referring to communication operations.

这里面有个容易被忽略的地方，就是有多个case同时符合条件时，Go将随机选择一个case进行执行。


下面的两个例子，case都有被执行的可能
```
package main

import (
	"fmt"
	"runtime"
	"time"
)

var ch1 = make(chan int)
var ch2 = make(chan int)
var chs = []chan int{ch1, ch2}
var numbers = []int{1, 2, 3, 4, 5}

func init() {
	runtime.GOMAXPROCS(runtime.NumCPU())
}
func main() {
	/*
	x := make(chan int, 2)
	y := make(chan int, 1)

	x <- 7
	x <- 8
	x <- 9
	y <- 9
	随机进入
	select {
	case e := <-x:
		fmt.Println("x -- selected", e)
	case <-y:
		fmt.Println("y -- selected")
	}*/

	//随机进入
	select {
	case getChan(0) <- getNumber(2):
		fmt.Println("1st case is selected.")
	case getChan(1) <- getNumber(3):
		fmt.Println("2st case is selected.")
	default:
		fmt.Println("default!.")
	}

	ch := make(chan int, 1024)
	go func(chan int) {
		for {
			val := <-ch
			fmt.Printf("val is: %d\n", val)
		}
	}(ch)
	tick := time.NewTicker(1 * time.Second)
	for i := 0; i < 20; i++ {
		select {
		case ch <- i:
		case <-tick.C:
			ch <- i
			fmt.Printf("%d:case<-tick\n", i)
		}

		time.Sleep(200*time.Millisecond)
	}
	close(ch)
	tick.Stop()
}

func getNumber(i int) int {
	fmt.Printf("numbers[%d]\n", i)
	return numbers[i]
}

func getChan(i int) chan int {
	fmt.Printf("chs[%d]\n", i)
	return chs[i]
}
```
