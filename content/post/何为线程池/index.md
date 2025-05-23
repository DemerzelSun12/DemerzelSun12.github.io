---
title: 何为线程池
draft: false
math: true
date: 2020-05-07 09:20:33
cover: images/featureimages/10.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

软件构造笔记-何为线程池
 ================
<!-- toc -->

 <!--more-->

# 一、什么是线程池

线程池，顾名思义就是装线程的池子。其用途是为了帮我们重复管理线程，避免创建大量的线程增加开销，提高响应速度。

# 二、为什么要使用线程池

规格严格的哈工大学生，不会希望别人看到我们的代码就开始吐槽，new Thread().start()会让代码看起来混乱臃肿，并且不好管理和维护，那么就需要用到线程池。
在编程中经常会使用线程来异步处理任务，但是每个线程的创建和销毁都需要一定的开销。如果每次执行一个任务都需要开一个新线程去执行，则这些线程的创建和销毁将消耗大量的资源；并且线程都是“各自为政”的，很难对其进行控制，更何况有一堆的线程在执行。线程池为我们做的，就是线程创建之后为我们保留，当我们需要的时候直接拿来用，省去了重复创建销毁的过程。

# 三、线程池的处理逻辑

## 3.1 线程池ThreadPoolExecutor构造函数
<center>
<img src="https://img-blog.csdnimg.cn/20200607104015880.png" width="80%" height="35%">
</center>
<center>
图3-1 ThreadPoolExecutor构造函数
</center>

简单介绍一下各个参数的用途
1. corePoolSize -> 该线程池中核心线程数最大值
核心线程：在创建完线程池之后，核心线程先不创建，在接到任务之后创建核心线程。并且会一直存在于线程池中（即使这个线程啥都不干），有任务要执行时，如果核心线程没有被占用，会优先用核心线程执行任务。数量一般情况下设置为CPU核数的二倍即可。
2. maximumPoolSize -> 该线程池中线程总数最大值
线程总数=核心线程数+非核心线程数
非核心线程：简单理解，即核心线程都被占用，但还有任务要做，就创建非核心线程
3. keepAliveTime -> 非核心线程闲置超时时长
这个参数可以理解为，任务少，但池中线程多，非核心线程不能白养着，超过这个时间不工作的就会被干掉，但是核心线程会保留。
4. TimeUnit -> keepAliveTime的单位
TimeUnit是一个枚举类型，其包括:
NANOSECONDS ： 1微毫秒 = 1微秒 / 1000
MICROSECONDS ： 1微秒 = 1毫秒 / 1000
MILLISECONDS ： 1毫秒 = 1秒 /1000
SECONDS ： 秒
MINUTES ： 分
HOURS ： 小时
DAYS ： 天
5. BlockingQueue workQueue -> 线程池中的任务队列
默认情况下，任务进来之后先分配给核心线程执行，核心线程如果都被占用，并不会立刻开启非核心线程执行任务，而是将任务插入任务队列等待执行，核心线程会从任务队列取任务来执行，任务队列可以设置最大值，一旦插入的任务足够多，达到最大值，才会创建非核心线程执行任务。

6. 常见的workQueue有四种：
   1. SynchronousQueue：这个队列接收到任务的时候，会直接提交给线程处理，而不保留它，如果所有线程都在工作怎么办？那就新建一个线程来处理这个任务！所以为了保证不出现<线程数达到了maximumPoolSize而不能新建线程>的错误，使用这个类型队列的时候，maximumPoolSize一般指定成Integer.MAX_VALUE，即无限大
   2. LinkedBlockingQueue：这个队列接收到任务的时候，如果当前已经创建的核心线程数小于线程池的核心线程数上限，则新建线程(核心线程)处理任务；如果当前已经创建的核心线程数等于核心线程数上限，则进入队列等待。由于这个队列没有最大值限制，即所有超过核心线程数的任务都将被添加到队列中，这也就导致了maximumPoolSize的设定失效，因为总线程数永远不会超过corePoolSize
   3. ArrayBlockingQueue：可以限定队列的长度，接收到任务的时候，如果没有达到corePoolSize的值，则新建线程(核心线程)执行任务，如果达到了，则入队等候，如果队列已满，则新建线程(非核心线程)执行任务，又如果总线程数到了maximumPoolSize，并且队列也满了，则发生错误，或是执行实现定义好的饱和策略
   4. DelayQueue：队列内元素必须实现Delayed接口，这就意味着你传进去的任务必须先实现Delayed接口。这个队列接收到任务时，首先先入队，只有达到了指定的延时时间，才会执行任务

7. ThreadFactory threadFactory -> 创建线程的工厂
可以用线程工厂给每个创建出来的线程设置名字。一般情况下无须设置该参数。

8. RejectedExecutionHandler handler -> 饱和策略
这是当任务队列和线程池都满了时所采取的应对策略，默认是AbordPolicy， 表示无法处理新任务，并抛出 RejectedExecutionException 异常。此外还有3种策略，它们分别如下。
   1. CallerRunsPolicy：用调用者所在的线程来处理任务。此策略提供简单的反馈控制机制，能够减缓新任务的提交速度。
   2. DiscardPolicy：不能执行的任务，并将该任务删除。
   3. DiscardOldestPolicy：丢弃队列最近的任务，并执行当前的任务。

<center>
<img src="https://img-blog.csdnimg.cn/20200607115033588.jpg" width="80%" height="35%">
</center>
<center>
图3-2 ThreadPoolExecutor构造函数总结
</center>

# 四、如何使用线程池

java为我们提供了4种线程池FixedThreadPool、CachedThreadPool、SingleThreadExecutor、ScheduledThreadPool，几乎可以满足大部分的需要了：

## 4.1 FixedThreadPool
可重用固定线程数的线程池，超出的线程会在队列中等待，在Executors类中可以找到创建方式：
<center>
<img src="https://img-blog.csdnimg.cn/20200607115737677.png" width="100%" height="35%">
</center>
FixedThreadPool的corePoolSize和maximumPoolSize都设置为参数nThreads，也就是只有固定数量的核心线程，不存在非核心线程。keepAliveTime为0L表示多余的线程立刻终止，因为不会产生多余的线程，所以这个参数是无效的。FixedThreadPool的任务队列采用的是LinkedBlockingQueue。

## 4.2 CachedThreadPool
CachedThreadPool是一个根据需要创建线程的线程池。

<center>
<img src="https://img-blog.csdnimg.cn/20200607120005472.png" width="100%" height="35%">
</center>

CachedThreadPool的corePoolSize是0，maximumPoolSize是Int的最大值，也就是说CachedThreadPool没有核心线程，全部都是非核心线程，并且没有上限。keepAliveTime是60秒，就是说空闲线程等待新任务60秒，超时则销毁。此处用到的队列是阻塞队列SynchronousQueue,这个队列没有缓冲区，所以其中最多只能存在一个元素,有新的任务则阻塞等待。

## 4.3 SingleThreadExecutor
SingleThreadExecutor是使用单个线程工作的线程池。其创建源码如下：
<center>
<img src="https://img-blog.csdnimg.cn/20200607135752753.png" width="100%" height="35%">
</center>
复制代码我们可以看到总线程数和核心线程数都是1，所以就只有一个核心线程。该线程池才用链表阻塞队列LinkedBlockingQueue，先进先出原则，所以保证了任务的按顺序逐一进行。

## 4.4 ScheduledThreadPool
ScheduledThreadPool是一个能实现定时和周期性任务的线程池，它的创建源码如下：
<center>
<img src="https://img-blog.csdnimg.cn/20200607140635943.png" width="100%" height="35%">
</center>
这里创建了ScheduledThreadPoolExecutor，继承自ThreadPoolExecutor，主要用于定时延时或者定期处理任务。ScheduledThreadPoolExecutor的构造如下：
<center>
<img src="https://img-blog.csdnimg.cn/20200607140949206.png" width="100%" height="35%">
</center>

可以看出corePoolSize是传进来的固定值，maximumPoolSize无限大，因为采用的队列DelayedWorkQueue是无解的，所以maximumPoolSize参数无效。该线程池执行如下：

<center>
<img src="https://img-blog.csdnimg.cn/20200607141247514.png" width="100%" height="35%">
</center>

当执行scheduleAtFixedRate或者scheduleWithFixedDelay方法时，会向DelayedWorkQueue添加一个实现RunnableScheduledFuture接口的ScheduledFutureTask(任务的包装类)，并会检查运行的线程是否达到corePoolSize。如果没有则新建线程并启动ScheduledFutureTask，然后去执行任务。如果运行的线程达到了corePoolSize时，则将任务添加到DelayedWorkQueue中。DelayedWorkQueue会将任务进行排序，先要执行的任务会放在队列的前面。在跟此前介绍的线程池不同的是，当执行完任务后，会将ScheduledFutureTask中的time变量改为下次要执行的时间并放回到DelayedWorkQueue中。


# 五、如何合理配置线程池的大小
一般需要根据任务的类型来配置线程池大小：
如果是CPU密集型任务，就需要尽量压榨CPU，参考值可以设为 NCPU+1
如果是IO密集型任务，参考值可以设置为2*NCPU
当然，这只是一个参考值，具体的设置还需要根据实际情况进行调整，比如可以先将线程池大小设置为参考值，再观察任务运行情况和系统负载、资源利用率来进行适当调整。
