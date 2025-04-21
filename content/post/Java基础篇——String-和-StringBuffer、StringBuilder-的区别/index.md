---
title: Java基础—String 和 StringBuffer、StringBuilder 的区别
draft: false
math: true
date: 2020-03-21 16:42:16
cover: images/featureimages/18.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---
软件构造笔记-Java基础—String 和 StringBuffer、StringBuilder 的区别
 ================
<!-- toc -->

 <!--more-->

# 一、三者简介
## 1.1 String
String 表示的就是 Java 中的字符串，日常用到的使用 "" 双引号包围的数都是字符串的实例。String 类其实是通过 char 数组来保存字符串的。下面是一个典型的字符串的声明
String s = "abc";
上面创建了一个名为 abc 的字符串。
字符串是Immutable的，一旦创建出来就不会被修改

## 1.2 StringBuffer
StringBuffer对象代表一个可变的字符串序列，当一个 StringBuffer 被创建以后，通过 StringBuffer 的一系列方法可以实现字符串的拼接、截取等操作。一旦通过 StringBuffer 生成了最终想要的字符串后，就可以调用其 toString 方法来生成一个新的字符串。StringBuffer 是线程安全的。

## 1.3 StringBuilder
StringBuilder 其实是和 StringBuffer 几乎一样，只不过 StringBuilder 是非线程安全的。并且，为什么 + 号操作符使用 StringBuilder 作为拼接条件而不是使用 StringBuffer 呢？我猜测原因是加锁是一个比较耗时的操作，而加锁会影响性能，所以 String 底层使用 StringBuilder 作为字符串拼接。

# 二、区别

## 2.1 可变性 
1. String 不可变
翻阅String源码，可以发现有一核心的属性 private final char value[];final很好地说明了String的不可变型，只可读，不可改。
对String进行任何的修改操作，如substring，replace，都会先复制一份String副本进行修改，在复制回堆区，相当于重新创建了一个String对象，原来的引用指向新的String。所以，在数据量很大的情况下，要不断进行复制操作，很占内存，这时要避免使用String。
2. StringBuilder 与 StringBuffer  可变
StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类
<center>
<img src="https://img-blog.csdnimg.cn/20200607183113887.png" width="80%" height="35%">
</center>

他们的构造函数执行时都会调用super()，调用父类的构造函数

<center>
<img src="https://img-blog.csdnimg.cn/20200607183230430.png" width="80%" height="35%">
</center>
char[] value 没有final修饰，说明它是一个可变对象，可以进行读和写操作。
线程安全性 
要验证这点非常容易，只要打开源码

<center>
<img src="https://img-blog.csdnimg.cn/20200607183335450.png" width="80%" height="35%">
</center>

StringBuffer对方法加了同步锁或者对调用的方法加了synchronized同步锁，所以是线程安全的。String，StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。

## 2.2 性能 
每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的String 对象。 
StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StirngBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

# 三、使用总结 

操作少量的数据 => String
单线程操作字符串缓冲区下操作大量数据 => StringBuilder
多线程操作字符串缓冲区下操作大量数据 => StringBuffer
