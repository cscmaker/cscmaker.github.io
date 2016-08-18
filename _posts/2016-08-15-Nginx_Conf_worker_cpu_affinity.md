---
layout:     post
title:      "Nginx Conf: worker_cpu_affinity探究"
author:     "Eric Cui"
header-img: "img/post-bg-05.jpg"
---
#### worker_porcesses & worker_cpu_affinity介绍
```
Syntax:worker_processes number | auto;
Default:worker_processes 1;
Context:main

Syntax:worker_cpu_affinity cpumask ...;
worker_cpu_affinity auto [cpumask];
Default:-
Context:main
```
* worker_processes: 指定worker进程的数量,一般指定CPU核数
* worker_cpu_affinity: 将worker进程与CPU集合进行绑定，CPU核用掩码表示

#### Linux下怎么看机器有多少核？

首先需要说明一下CPU物理个数、逻辑个数的概念。CPU物理个数是只机器上的CPU卡槽上CPU实体的数量，这个数量是真实的CPU个数。
单个物理CPU上有多个能处理数据的芯片组，就是常说的是（双核、四核等）。
所以一般情况下，逻辑CPU个数 = 物理CPU个数*单个CPU核数。
但是有的服务器会支持并开启超线程（Hyper-Threading）技术,这种情况下 逻辑CPU个数 = 物理CPU个数*单个CPU核数*2

命令方式查看Linux下CPU个数：
```
物理个数：
grep "physical id" /proc/cpuinfo |sort|uniq|wc -l

CPU核数：
grep "cpu cores" /proc/cpuinfo|sort|uniq

逻辑CPU核数：
grep "processor" /proc/cpuinfo|wc -l
```
<br/>
#### 为什么要绑定？
为每个worker进程绑定指定的CPU内核，这样单个进程就可以独享这个CPU（假设绑定关系是一对一），避免了多个worker进程同时抢同一个CPU的情况，实现了
内核调度策略上的真正并发,充分利用SMP（对称多处理结构）多核处理架构。<br/>
more info: [SMP](http://www-31.ibm.com/support/techdocs/cn/faqhtmlfaq/2511084C28000.htm)
