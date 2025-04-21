---
title: 不能使用泛型的几种情况
draft: false
math: true
date: 2020-04-28 15:46:00
cover: images/featureimages/1.jpg
summary: Java学习——不能使用泛型的几种情况
tags: [软件构造,笔记]
categories: ["软件构造"]
---


软件构造笔记-Java 中不能使用泛型的几种情况
 ================
<!-- toc -->

 <!--more-->

# 一、前言
Java 1.5 引入了泛型来保证类型安全，防止在运行时发生类型转换异常，让类型参数化，提高了代码的可读性和重用率。但是有些情况下泛型也是不允许使用的，总结一下编码中不能使用泛型的一些场景。

# 二、基本类型无法直接使用泛型
以下写法是错误的：
<center>
<img src="https://img-blog.csdnimg.cn/20200607154813512.png" width="80%" height="35%">
</center>
基本类型是不能够作为泛型类型的,需要使用它们对应的包装类。
<center>
<img src="https://img-blog.csdnimg.cn/20200607154909119.png" width="80%" height="35%">
</center>

# 三、泛型类型无法被直接实例化
泛型类型可以理解为一个抽象类型，只是代表了类型的抽象，因此不能直接实例化它，下面的做法也是错误的：
<center>
<img src="https://img-blog.csdnimg.cn/20200607155137663.png" width="80%" height="35%">
</center>

# 四、泛型无法作为静态变量类型
Java 中的静态类型随着类加载而实例化，此时泛型的具体类型并没有声明。同时因为静态变量作为所有对象的共享变量，只有类实例化或者方法调用时才能确定其类型。如果是泛型类型将无法确定其类型。同样在类上声明的泛型也无法作为返回值类型出现在类的静态方法中，下面的写法也是错误的：
<center>
<img src="https://img-blog.csdnimg.cn/20200607155539892.png" width="80%" height="35%">
</center>


# 五、无法进行 instanceof 判断
Java 中的泛型是伪泛型，在编译期会被擦除，运行的字节码中不存在泛型，所以下面的判断条件无法进行：
<center>
<img src="https://img-blog.csdnimg.cn/202006071607517.png" width="80%" height="35%">
</center>

但是泛型的无界通配符 <?> 可以进行 instanceof 判断，你仔细想想为什么。

# 六、无法创建参数化类型的数组
首先下面这种写法是对的：
<center>
<img src="https://img-blog.csdnimg.cn/20200607160921704.png" width="80%" height="35%">
</center>
但是加上了泛型就编译不通过了：
<center>
<img src="https://img-blog.csdnimg.cn/20200607161025384.png" width="80%" height="35%">
</center>

# 七、不能直接或者间接扩展Throwable
下面的两种写法将引发编译错误：
<center>
<img src="https://img-blog.csdnimg.cn/20200607161123191.png" width="80%" height="35%">
</center>

如果成立将出现：
<center>
<img src="https://img-blog.csdnimg.cn/20200607161218343.png" width="80%" height="35%">
</center>

如何才能对异常进行具体的处理，这显然不便于精确的异常处理逻辑。但是可以抛出一个 不确定的异常，但是同样不能在静态方法中使用类声明的泛型：
<center>
<img src="https://img-blog.csdnimg.cn/20200607161412985.png" width="80%" height="35%">
</center>

# 八、泛型擦除后相同参数签名的方法不能重载
由于泛型擦除的原因，以下的不视为方法的重载且无法编译 ：

<center>
<img src="https://img-blog.csdnimg.cn/2020060716150451.png" width="80%" height="35%">
</center>
