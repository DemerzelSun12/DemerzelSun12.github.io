---
title: 软件构造-第三章-第五节-ADT和OOP中的“等价性”
date: 2020-04-08 16:28:25
draft: false
math: true
cover: images/featureimages/7.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

软件构造第三章 第五节 ADT和OOP中的“等价性”
 ================
<!-- toc -->

 <!--more-->

# 一、等价性equals() 和 ==

## 1.1 等价关系

1. 等价关系是指对于关系E ⊆ T x T ，满足：
   - 自反性: x.equals(x)必须返回true
   - 对称性: x.equals(y)与y.equals(x)的返回值必须相等。
   - 传递性: x.equals(y)为true，y.equals(z)也为true，那么x.equals(z)必须为true。

2. 从使用者/外部的角度去观察：
   - 我们说两个对象相等，当且仅当使用者无法观察到它们之间有不同，即每一个观察总会都会得到相同的结果。例如对于两个集合对象 {1,2} 和 {2,1}，我们就无法观察到不同：
     - |{1,2}| = 2, |{2,1}| = 2
     - 1 ∈ {1,2} is true, 1 ∈ {2,1} is true
     - 2 ∈ {1,2} is true, 2 ∈ {2,1} is true
     - 3 ∈ {1,2} is false, 3 ∈ {2,1} is false

## 1.2 ==与equals()方法

1. ==是引用等价性
   - 比较的是索引。更准确的说，它测试的是指向相等（referential equality）。如果两个索引指向同一块存储区域，那它们就是==的。对于之前提到过的快照图来说，==就意味着它们的箭头指向同一个对象。

2. equals()是对象等价性
   - equals()操作比较的是对象的内容，换句话说，它测试的是对象值相等（object equality）。e在每一个ADT中，quals操作必须合理定义。
   - 在自定义ADT时，需要重写Object的equals()方法，因为在Object中实现的缺省equals()是在判断引用等价性，如图所示。
   - 当重写equals()方法时，应使用@Override注释。Java编译器将检查超类中是否确实存在具有相同签名的方法，如果在签名中犯了错误，则会给出一个编译器错误。

<center>
<img src="https://img-blog.csdnimg.cn/20200418210227131.jpg" width="50%" height="35%">
</center>
<center>
图1-1 Object类中的equals()方法
</center>
<br>

## 1.3 instanceof

1. instanceof运算符
   - instanceof是Java中的二元运算符，左边是对象，右边是类；当对象是右边类或子类所创建对象时，返回true；否则，返回false。
   - 使用instanceof是动态检查，而非静态检查
   - 不要在父类型中使用instanceof检查子类型。

2. 使用注意事项
   - 左边的对象实例不能是基础数据类型
   - 左边的对象实例和右边的类不在同一个继承树上
   - null用instanceof跟任何类型比较时都是false

## 二、hashCode()方法

## 2.1 不可变类型

1. equals() 应该比较抽象值是否相等。这和 equals() 比较行为相等性是一样的。
2. hashCode() 应该将抽象值映射为整数。
3. 所以不可变类型应该同时覆盖 equals() 和 hashCode()。

## 2.2 对于可变类型：

1. equals() 应该比较索引，就像 ==一样。同样的，这也是比较行为相等性。
2. hashCode() 应该将索引映射为整数。
3. 所以可变类型不应该将 equals() 和 hashCode() 覆盖，而是直接继承 Object中的方法。Java没有为大多数聚合类遵守这一规定，这也许会导致上面看到的隐秘bug。

## 2.3 equals()与hashcode()区别

1. equals与hashCode两个方法均属于Object对象，equals根据我们的需要重写， 用来判断是否是同一个内容或同一个对象，具体是判断什么，怎么判断得看怎么重写，默认的equals是比较地址。
2. hashCode方法返回一个int的哈希码， 同样可以重写来自定义获取哈希码的方法。
3. equals判定为相同， hashCode一定相同。equals判定为不同，hashCode不一定不同。
4. hashCode必须为两个被该equals方法视为相等的对象产生相同的结果。
5. 与equals()方法类似，hashCode()方法可以被重写。JDK中对hashCode()方法的作用，以及实现时的注意事项做了说明：
   - hashCode()在哈希表中起作用，如java.util.HashMap。
   - 如果对象在equals()中使用的信息都没有改变，那么hashCode()值始终不变。
   - 如果两个对象使用equals()方法判断为相等，则hashCode()方法也应该相等。
   - 如果两个对象使用equals()方法判断为不相等，则不要求hashCode()也必须不相等；但是开发人员应该认识到，不相等的对象产生不相同的hashCode可以提高哈希表的性能。
