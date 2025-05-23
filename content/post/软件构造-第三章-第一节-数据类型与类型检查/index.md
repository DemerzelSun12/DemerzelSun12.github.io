---
title: 软件构造-第三章-第一节-数据类型与类型检查
date: 2020-03-24 11:35:19
draft: false
math: true
cover: images/featureimages/3.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

软件构造第三章 第一节 数据类型与类型检查
 ================
<!-- toc -->

 <!--more-->

# 一、数据类型

## 1.1 类型和变量

1. 数据类型：
   - 基本数据类型
&emsp;``` int double long boolean double char ```
   - 面向对象的数据类型
&emsp;``` String BigInteger```
对于Java，基本数据类型首字母小写，面向对象数据类型首字母大写。
基本数据类型直接在栈中存值，不存变量名；面向对象数据类型在堆中存储，运行时在堆中为其分配内存，不仅值，也存变量名。

<center>
<img src="https://img-blog.csdnimg.cn/20200331183349392.png" width="80%" height="35%">
</center>
<center>
图1-1 基本数据类型与面向对象的数据类型差异
</center>
<br>


# 二、静态检查、动态检查

## 2.1 静态检查VS动态检查

&emsp;&emsp;Java是一种静态类型的语言；所有变量的类型在编译的时候就已经知道了（程序还没有运行），所以编译器也可以测出每一个表达式的类型。在动态类型语言中（例如Python），这种类型检查是发生在程序运行的时候。
</br>

## 2.2 静态检查：关于“类型”的检查

&emsp;&emsp;&emsp;检查严格程度：静态检查>>动态检查>>无检查。

1. 在编译阶段发现错误，避免将错误带入到运行阶段，提高程序的正确性\健壮性

2. 静态分析检查的类型
     * 语法错误，例如多余的标点符号或者错误的关键词。即使在动态类型的语言例如Python中也会做这种检查：如果你有一个多余的缩进，在运行之前就能发现它。
     * 类名\函数名错误，例如Math.sine(2) . (应该是 sin )
     * 参数数目错误，例如 Math.sin(30, 20) 
     * 参数的型错误 Math.sin("30") 
     * 返回值类型错误 ，例如⼀个声明返回 int 类型函数 return "30”
</br>

## 2.3 动态检查：关于“值”的检查

&emsp;&emsp;&emsp;bug在运行中被发现。

1. 倾向于检查特定值才出发的错误
2. 动态分析检查的类型：
     * 非法的变量值。例如整型变量x、y，表达式x/y 只有在运行后y为0才会报错，否则就是正确的。
     * 非法的返回值。例如最后得到的返回值无法用声明的类型来表明。
     * 越界访问。例如在一个字符串中使用一个负数索引。
     * 空指针，使用一个null 对象解引用。


# 三、Mutability & Immutability
&emsp;&emsp;改变一个变量：是将该变量指向另一个值得存储空间
&emsp;&emsp;改变一个变量的值：是将该变量当前指向的值的存储空间中写入一个新的值

## 3.1 Immutability(不变性)
1. final 变量能被显式地初始化并且只能初始化一次。不变数据类型，一旦被创建，值不可修改；
2. 基本类型及其封装对象类型都是不可变的
3. 不可变的引用是指一旦指定引用位置后，不可再次指定。
4. 如果编译器不能确定final变量不会改变，就提示错误，这也是静态类型检查的一部分
5. 尽量使用final变量作为方法的输入参数、作为局部变量。
6. 注意：
   - final类无法派生子类
   - final变量无法改变值/引用
   - final方法无法被子类重写

## 3.2 Mutability(可变性)
1. 不变对象：一旦被创建，始终指向同个值/引用 
2. 可变对象：拥有方法以修改自己的值/引用
3. Sting与StingBuilder
- String：不可变数据类型，在修改时必须创建一个新的String对象

```
String s = "a";
a = s + "b";//s = s.concat("b");
```

- StringBuilder:可改变的数据类型，可以直接修改对象的值
```
StringBuilder sb = new StringBuilder("a");
sb.append("b");
```
## 3.3 可变性与不可变性的优缺点
1. 可变数据类型最小化的拷贝以提高效率；使用 不可变类型，对其频繁修改会产生大量的临时拷贝 (需要垃圾回收 )
2. 可变数据类型，可获得更好的效能
3. 可变数据类型也适合在多个模块之间共享数据
4. 不可变数据类型更安全，更易于理解，也更方便改变

## 3.4 防御性拷贝

&emsp;&emsp;如果一个方法或构造函数允许可变对象进/出，那么就要考虑一下使用者是否有可能改变它。如果是的话，那你必须对该对象进行保护性拷贝，使进入方法内部的对象是外部时的拷贝而不它本身（因为外部的对象有可能还会被改变）。

# 四、Snapshot diagram(快照图)

1. 基本类型的值：原始值由裸露的常量表示。传入箭头引用变量或对象字段的值。
<center>
<img src="https://img-blog.csdnimg.cn/20200331213946664.png" width="80%" height="35%">
</center>
<center>
图4-1 基本类型快照图
</center>
<br>

2. 对象类型的值：一个对象值是一个由它的类型标记的圆。当我们想要显示更多的细节时，我们在它里面写字段名称，箭头指向他们的值。 
<center>
<img src="https://img-blog.csdnimg.cn/20200331214137439.png" width="80%" height="35%">
</center>
<center>
图4-2 对象类型快照图
</center>
<br>

3. 不可变对象：用双线椭圆。
<center>
<img src="https://img-blog.csdnimg.cn/2020033121430139.png" width="80%" height="35%">
</center>
<center>
图4-3 不可变对象快照图
</center>
<br>

4. 不可变的引用：用双线箭头
   - 引用时不可变的，但指向的值可以是可变的
   - 可变的引用，也可指向不可变的值
<center>
<img src="https://img-blog.csdnimg.cn/20200331214218759.png" width="80%" height="35%">
</center>
<center>
图4-4 不可变引用快照图
</center>
<br>