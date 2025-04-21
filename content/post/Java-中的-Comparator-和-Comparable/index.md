---
title: Java 中的 Comparator 和 Comparable
draft: false
math: true
date: 2020-04-07 15:06:58
cover: images/featureimages/21.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

软件构造笔记-Java 中的 Comparator 和 Comparable
 ================
<!-- toc -->

 <!--more-->

# 一、 前言

最近看到了集合排序（基于 Java 8），学到了一种基于Stream的排序，排序可以这么写：
<center>
<img src="https://img-blog.csdnimg.cn/20200607151054681.png" width="80%" height="35%">
</center>

这里排序用到了一个关键接口 java.util.Comparator。排序比较作为业务中经常出现的需求，我们有必要研究一下这个接口。

# 二、 Comparator 概念

Comparator 是一个函数式接口。它经常用于没有天然排序的集合进行排序，如 Collections.sort 或 Arrays.sort。或者对于某些有序数据结构的排序规则进行声明，如 TreeSet 、TreeMap 。也就是该接口主要用来进行集合排序。

# 三、Comparator 中的方法
Comparator 作为一个函数式接口只有一个抽象方法,但是它有很多的默认方法。

## 3.1 compare 抽象方法
作为Comparator 唯一的抽象方法，int compare(T o1,T o2) 比较两个参数的大小， 返回负整数，零，正整数 ，分别代表 o1 < o2、o1=o2、o1 > o2，通常分别返回 -1、0 或 1。伪表达式：
```
// 输入两个同类型的对象 ，输出一个比较结果的int数字
(x1,x2)-> int
```
实现该方法一定要注意以下事项：

- 必须保证compare(x,y) 和compare(y,x) 的值的和必须为 0 。
- 必须保证比较的顺序关系是可传递的，如果compare(x,y)>0 而且compare(y,z)>0 则 compare(x,z)>0。
- 如果存在 compare(x,y)=0，则对于 z 而言，存在 compare(x, z)==compare(y, z)。

然而并不严格要求(compare(x, y)==0) == (x.equals(y))。一般说来，任何违背这个条件的 Comparator 实现都应该明确指出这一事实情况。

## 3.2 comparing 系列方法
从 Java 8 开始，Comparator 提供了一系列的静态方法，并通过函数式的风格赋予 Comparator 更加强大和方便的功能，暂且称它们为 comparing系列方法。
<center>
<img src="https://img-blog.csdnimg.cn/20200607151054681.png" width="80%" height="35%">
</center>
该方法是该系列方法的基本方法。来分析一下该方法。它一共两个参数都是函数式接口。
第一个参数 Function < ? super T, ? extends U > keyExtractor 表示输入一个是 T 类型对象，输出一个 U 类型的对象，举个例子，输入一个 People 对象返回其年龄 Integer 数值：
//   people -> people.getAge(); 转换为下面方法引用
Function < People, Integer > getAge = People::getAge;
第二个参数 keyComparator就很好理解了，表示使用的比较规则。
对 c1,c2 按照 第一个参数 keyExtractor 提供的规则进行提取特征，然后第二个参数keyComparator对这两个特征进行比较。
Comparator & Serializable 为 Java 8 新特性:同时满足这两个类型约束

理解了这个方法后，其它该系列的方法就好理解了，这里不再赘述。目前 comparing 系列方法使用更加广泛。举一些例子：
<center>
<img src="https://img-blog.csdnimg.cn/20200607153811907.png" width="90%" height="35%">
</center>
可以使用 java.util.Collections 或者 Stream 提供的排序方法来使用Comparator。

# 四、小结
对 Comparator进行了简单的分析，它用于构建集合排序的规则，在日常使用中非常有用。
