---
layout:     post
title:      "Nginx Conf: worker_cpu_affinity探究"
author:     "Eric Cui"
header-img: "img/post-bg-05.jpg"
---
### worker_porcesses & worker_cpu_affinity介绍
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

### Linux下怎么看机器有多少核？
首先需要说明一下CPU物理个数、逻辑个数的概念。CPU物理个数是只机器上的CPU卡槽上CPU实体的数量，这个数量是真实的CPU个数。

--未完待续……
