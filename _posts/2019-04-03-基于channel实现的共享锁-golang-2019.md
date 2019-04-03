---
layout:     post
title:      "golang-基于channel实现的共享锁"
subtitle:   " \"golang共享锁\""
date:       2019-04-03 11:05:13
author:     "Tommy"
header-img: "img/post-bg-go.jpg"
catalog: true
tags:
    - Golang
    - Channel
    - Semaphore
---

## 问题背景
在实际项目中，希望用n个goroutine去一个带缓冲区的channel中读取数据，并且每个goroutine每次消费1000个数据，如果不足则按具体个数消费即可。为什么每次要消费1000个数据呢，因为希望在每个goroutine里面使用redis pipeline去获取这1000个数据的对应信息，这样就可以减少redis的访问次数。 而Channel中的数据量是未知的有限数据。

## 解决思路
在上述的背景下，因为本人之前是Java开发者，所以想到了Java里面的Semaphore共享锁，共享锁就是可以一个有n个凭证去访问共享资源的共享锁，是不是一说这里，大家就都想到了解决办法，n个goroutine并行执行，其实就相当于有n个凭证可以去访问共享资源。所以下面我们要做的就是用go去实现一个共享锁，这样问题就解决了。 

## 具体实现
go实现共享锁，我用了一种比较简单的方式，借助缓冲区大小为n的channel去实现，当缓冲区个数达到n个之后，channel会阻塞。所以每启动一个goroutine，就向channel里面插入一个数据，每个goroutine完成，channel就输出一个数据，这样可以保证程序中最多只有n个goroutine在运行。下面看下代码具体实现。<br/>

`semaphore.go`

```

// Semaphore 借助channel实现的共享锁
type Semaphore struct {
    Channel chan int
}

// NewSemaphore 创建一个共享锁 有parallelism个共享凭证
func NewSemaphore(parallelism int) *Semaphore {
    return &Semaphore{Channel: make(chan int, parallelism)}
}

// Acquire 获取一个凭证
func (s *Semaphore) Acquire() {
    s.Channel <- 1
}

// Release 归还一个凭证
func (s *Semaphore) Release() {
    <-s.Channel
}
```

定义一个chan int类型，用来缓冲凭证个数，每获取一个凭证，就向channel里面加入一个int数据。<br/>
当一个goroutine运行完成之后，通过Release方法归还一个凭证，即channel输出一个数据，这样channel就不阻塞了，
此时就又可以启动一个新的goroutine了。具体的实现逻辑大概就是这样的了。

## 测试案例
下面通过一个小程序来测试一个整个流程。<br/>

`main.go`

```

import (
    "fmt"
    "sync"
    "time"

    "jianshu.com/mozi/semaphore"
)

func main() {
    data := make(chan int, 1000)
    go func() {
        defer close(data) //一定要close channel
        for i := 1; i < 11000; i++ {
            data <- i
            time.Sleep(1)
        }
    }()
    g := sync.WaitGroup{}
    sem := semaphore.NewSemaphore(5)
    flag := false //用来标志是否跳出外围死循环

    i := 0 //在运行的goroutine个数

    for {
        sem.Acquire()
        g.Add(1)
        i++
        fmt.Println(fmt.Sprintf("goroutine-%d", i))
        go func() {
            defer g.Done()
            defer sem.Release() //defer感觉可以理解为java中的finally
            var items []int
            for item := range data {
                items = append(items, item)
                if len(items) == 1000 {
                    break
                }
            }
            if len(items) == 0 {
                flag = true
            }
            consumer(items)
            i--
        }()

        if flag {
            break
        }
    }

    g.Wait()
    fmt.Println("exit program")
}

func consumer(data []int) {
    for _, v := range data {
        if v%1000 == 0 {
            fmt.Println(v)
        }
    }
}
```

定义了1个缓冲区为1000的channel去存储数据，然后在每个goroutine里面的去消费数据，将能整除1000的数打印出来，每个goroutine每次处理1000个数据。<br/>
通过Smaphore共享锁控制程序中最多有5个goroutine在执行task。<br/>
通过len(items) == 0，判断 channel中的数据是否消费完了。

## 运行结果
<img src = "/img/golang/semaphore-go.png">

通过运行结果可以看出，程序中每次确实最多有5个goroutine在并行。 最后消费完所有数据后,退出程序。<br/>
也可以看到输出中确实没问题，1000到10000都正常输出。都是1000的倍数。