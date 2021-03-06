---
title: 几个概念：并发、并行、进程、线程和协程
date: 2017-01-20 10:11:19
tags: [Concurrency, Parallelism, Coroutine]
---

“有了协程框架再也不用关注线程池调度问题”，在阿里双十一电子书[《不一样的技术创新》](https://102.alibaba.com/newsInfo.htm?newsId=28&channel=127)中看到这样一句话。<!-- more -->看到这句话的时候，内心活动是这样的——当我们还在玩线程池的时候，阿里的爸爸们已经在研究调度问题并有解决方案了。所以保持对这句话的质疑和思考，有了今天的整理和学习，为进一步学习协程框架，如[Akka](http://akka.io/)起一个头。

## 并发和并行

在多线程编程中，并发（Concurrency）和并行（Parallelism）这两个概念时常会被提到，但是这两个概念却不是一个意思。

### 并发（Concurrency）

并发指的是应用程序同时处理多个任务，多个任务都能同时取得进展。
![](http://elbarco.eos.eayun.com/imgs/concurrency-vs-parallelism-1.png)

比如吃饭的时候，电话来了，停下手和嘴去接电话，打完电话继续吃饭，这叫并发。

### 并行（Parallelism）

并行指的是应用可以将任务拆成更小的子任务，而这些子任务可以同时平行着被处理。
![](http://elbarco.eos.eayun.com/imgs/concurrency-vs-parallelism-2.png)

比如吃饭的同时，我还能打电话，这就是并行。

### 对比

由上面可以看出，并发依赖于应用如何处理多任务。应用同时只能处理一个任务，处理完在进行下一个任务，这叫顺序地（Sequentially）执行；如果应用同时能处理多个任务，这就叫并发地。

另一方面，并行，则依赖于应用如何处理每个独立的任务。应用可以顺序的将任务从头至尾的执行完，也可以将任务分解成子任务，而子任务可以平行执行。

并发与顺序相对，而并行是并发的子集。

这里还有一个更形象的例子：
![](http://elbarco.eos.eayun.com/imgs/con_and_par.jpg)


## 进程、线程和协程

### 进程（Process）

是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础。在当代面向线程设计的计算机结构中，进程是线程的容器。程序是指令、数据及其组织形式的描述，进程是程序的实体。

### 线程（Thread）

线程，有时被称为轻量级进程(Lightweight Process，LWP），是程序执行流的最小单元。一个标准的线程由线程ID，当前指令指针(PC），寄存器集合和堆栈组成。

### 协程（Coroutine, Fiber）

协程，又称为微线程，在Lua、Python、Go中有所体现。这里参考[廖雪峰的文章](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868328689835ecd883d910145dfa8227b539725e5ed000)，举个例子：
如果有两个子程序A和B：
```python
def A():
    print '1'
    print '2'
    print '3'

def B():
    print 'x'
    print 'y'
    print 'z'
```

对于这两个子程序，一次调用，一次返回，返回结果很有可能是：
```
1
2
3
x
y
z
```
而协程看上去虽然也是子程序，但是在执行过程中，在子程序内部可以中断，然后转而执行别的子程序，在适当的时候在返回来接着执行。比如上面的两个子程序假设由协程执行，那么再执行A的过程中，可以随时终端，去执行B，B也有可能在执行过程中中断再去执行A，结果有可能是：
```
1
2
x
y
3
z
```
有些类似多线程，但是协程的特点实在一个线程中执行。其优势就是具有极高的执行效率，因为执行过程中不需要县城切换，减少了CPU切换线程的开销。另一方面，避免了多线程的锁机制，不会存在同时的写冲突。

### Java与协程

Java语言本身不支持协程，通过第三方的库、协程框架可以实现，如注明的akka，kilim等。





