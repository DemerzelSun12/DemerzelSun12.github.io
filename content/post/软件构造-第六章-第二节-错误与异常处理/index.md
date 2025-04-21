---
title: 软件构造-第六章-第二节-错误与异常处理
date: 2020-05-16 16:13:47
draft: false
math: true
cover: images/featureimages/15.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---
软件构造第六章 第二节 错误与异常处理
 ================
<!-- toc -->

 <!--more-->

# 一、JAVA中的错误和异常

## 1.1 Throwable
1. Throwable 类是 Java 语言中所有错误或异常的超类。
2. 继承的类：extends Object。
3. 实现的接口：implements Serializable。
4. 直接已知子类：Error, Exception（直接已知子类：IOException、RuntimeException）。
<center>
<img src="https://img-blog.csdnimg.cn/20200712194922867.png" width="70%" height="25%">
</center>
<center>
图1-1 Throwable接口
</center>

## 1.2 Error
1. Error类描述很少发生的Java运行时系统内部的系统错误和资源耗尽情况（例如，VirtualMachineError，LinkageError）。
2. 对于内部错误：程序员通常无能为力，一旦发生，想办法让程序优雅的结束
3. Error的类型：
   - 用户输入错误
     - 例如：用户要求连接到语法错误的URL，网络层会投诉。
   - 设备错误
     - 硬件并不总是做你想做的。
     - 输出器被关闭
     - 物理限制
     - 磁盘可以填满
     - 可能耗尽了可用内存

# 二、异常的处理

## 2.1 异常按结构层次的分类
1. 运行时异常：由程序员处理不当造成，如空指针、数组越界、类型转换
2. 其他异常：程序员无法完全控制的外在问题所导致的，通常为IOE异常，即找不到文件路径等

## 2.2 异常按处理机制角度的分类
&emsp;&emsp;区分checked 和 unchecked：编译器将检查是否为所有的已检查异常提供了异常处理机制，比如说我们使用Class.forName()来查找给定的字符串的class对象的时候，如果没有为这个方法提供异常处理，编译是无法通过的。

### 2.2.1 Checked exception：
1. 编译器可帮助检查你的程序是否已抛出或处理了可能的异常
异常的向上抛出机制进行处理，如果子类可能产生A异常，那么在父类中也必须throws A异常。可能导致的问题：代码效率低，耦合度过高。
checked exception是需要强制catch的异常，你在调用这个方法的时候，你如果不catch这个异常，那么编译器就会报错，比如说我们读写文件的时候会catch IOException，执行数据库操作会有SQLException等。
2. 对checked Exception处理机制　　　　
   - 抛出：声明是throws，抛出时throw　　　　
   - 捕获（try/catch）：try出现异常，忽略后面代码直接进入catch；无异常不进入catch；若catch中没有匹配的异常处理，程序退出；若子类重写了父类方法，父类方法没有抛出异常，子类应自己处理全部异常而不再传播；子类从父类继承的方法不能增加或更改异常
   - 处理：不能代替简单的测试，尽量苛刻、不过分细化、将正常处理与异常处理分开、利用好层次结构、早抛出晚捕获、避免不必要的检查
   - 清理现场、释放资源（finally）：finally中语句不论有无异常都执行

### 2.2.2 Unchecked exception:
1. 程序员对此不做任何事情，不得不重写你的代码（不需要在编译时使用try-catch等机制处理）
2. 这类异常都是RuntimeException的子类，它们不能通过client code来试图解决
3. 这种异常不是必须需要catch的，你是无法预料的，比如说你在调用一个 list.szie()的时候，如果这个list为null，那么就会报NUllPointerException，而这个异常就是 RuntimeException，也就是UnChecked Exception
4. 常见的unchecked exception：JVM抛出，如空指针、数组越界、数据格式、不合法的参数、不合法的状态、找不到类等

# 三、checked异常的处理机制
## 3.1 异常中的LSP原则

1. 如果子类型中override了父类型中的函数，那么子类型中方法抛出的异常不能比父类型抛出的异常类型更广泛
   - 子类型方法可以抛出更具体的异常，也可以不抛出任何异常
   - 如果父类型的方法未抛出异常，那么子类型的方法也不能抛出异常。
   - 其他的参考第五章第二节的LSP
2. 利用throws进行声明
   - 使用throws声明异常：此时需要告知你的client需要处理这些异常，如果client没有handler来处理被抛出的checked exception，程序就终止执行。
   - 程序员必须在方法的spec中明确写清本方法会抛出的所有checked exception，以便于调用该方法的client加以处理
   - 在使用throws时，方法要在定义和spec中明确声明所抛出的全部checked exception，没有抛出checked异常，编译出错，Unchecked异常和Error可以不用处理。

## 3.2 利用throw抛出一个异常
1. 步骤：
   - 找到一个能表达错误的Exception类/或者构造一个新的Exception类
   - 构造Exception类的实例，将错误信息写入抛出它
   - 一旦抛出异常，方法不会再将控制权返回给调用它的client，因此也无需考虑返回错误代码
 2. try-catch语句
   - 使用 try 和 catch 关键字可以捕获异常。try/catch 代码块放在异常可能发生的地方。
   - try/catch代码块中的代码称为保护代码，
   - Catch 语句包含要捕获异常类型的声明。当保护代码块中发生一个异常时，try 后面的 catch 块就会被检查。
   - 如果发生的异常包含在 catch 块中，异常会被传递到该 catch 块，这和传递一个参数到方法是一样。
3. finally语句
   - 场景：当异常抛出时，方法中正常执行的代码被终止；但如果异常发生前曾申请过某些资源，那么异常发生后这些资源要被恰当的清理，所以需要用finally语句。
   - finally 关键字用来创建在 try 代码块后面执行的代码块。
   - 无论是否发生异常，finally 代码块中的代码总会被执行。
   - 在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。
   - finally 代码块出现在 catch 代码块最后：
4. 注意下面事项：
   - catch 不能独立于 try 存在。
   - 在 try/catch 后面添加 finally 块并非强制性要求的。
   - try 代码后不能既没 catch 块也没 finally 块。
   - try, catch, finally 块之间不能添加任何代码。

# 四、自定义异常
&emsp;&emsp;如果JDK提供的exception类无法充分描述你的程序发生的错误，可以创建自己的异常类。
   - 如果想写一个检查性异常类，则需要继承 Exception 类。
   - 如果想写一个运行时异常类，那么需要继承 RuntimeException 类。