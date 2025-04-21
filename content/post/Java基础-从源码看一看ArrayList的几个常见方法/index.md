---
title: Java基础 | 从源码看一看ArrayList的几个常见方法
draft: false
math: true
date: 2020-03-03 15:29:37
cover: images/featureimages/16.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

# 从源码看一看ArrayList的几个常见方法

## 第一节 ArrayList简介
 - ArrayList是一个数组队列，相当于动态数组。与Java中的数组相比，它的容量能动态增长。它继承于AbstractList，实现了List，RandomAccess[随机访问]，Cloneable[可克隆]，java.io.Serializable[序列化]这些接口。ArrayList继承了AbstractList，实现了List，提供了相关的添加、删除、修改、遍历等功能。
 - ArrayList实现了RandmoAccess接口，即提供了随机访问功能。RandmoAccess是java中用来被List实现，为List提供快速访问功能的。在ArrayList中，可以通过元素的序号快速获取元素对象，即快速随机访问。
 - ArrayList 实现了Cloneable接口，即覆盖了函数clone()，能被克隆。ArrayList   实现java.io.Serializable接口，这意味着ArrayList支持序列化，能通过序列化去传输。
 - 与Vector不同，ArrayList中的操作不是线程安全的。所以，建议在单线程中才使用ArrayList，而在多线程中可以选择Vector或者CopyOnWriteArrayList。

 <!--more-->

## 第二节 增删查改源代码解析
### 2.1 增加
&emsp;&emsp;ArrayList添加元素的操作，这里介绍两个方法 `add(E object)` 和 `add(int index, E object)` 。其他的方法，类似`addAll(int index, Collection<? extends E> c)`等，自行探索即可。

 - add(E object)方法
&emsp;&emsp;add(E object)方法的结果是在list末尾增添新元素，源码如图2-1所示。
<center>
<img src="https://img-blog.csdnimg.cn/20200229142829130.png" width="70%" height="25%">
</center>
<center>
图2-1 add(E object)方法源码
</center>

 　　　　　　　　　　
&emsp;&emsp;可以看到add()方法首先使私有属性modCount自增。然后调用私有方法`add(E e, Object[] elementData, int s)`，最后返回。`add(E e, Object[] elementData, int s)`方法首先检查List长度，若目前List长度等于size，则会调用grow()方法进行扩容。grow()方法如图2-2所示：

<center>
<img src="https://img-blog.csdnimg.cn/20200229190846990.png" width="70%" height="25%">
</center>

<center>图2-2 grow()方法
</center>

&emsp;&emsp;如果原有List不空，旧List长度加一，并进行新旧List的复制；若原有List长度为0，就new一个新List。
&emsp;&emsp;调用grow()方法之后，将新加入的元素放置在末尾，List的size属性自增。

 - add(int index, E object)方法
 	add(int index, E object)是指在 index的位置 ，插入一个新元素，源码如图2-3所示。
 <center>
<img src="https://img-blog.csdnimg.cn/20200229191922638.png" width="70%" height="25%">
</center>
<center>
图2-3 add(int index, E object)方法
</center>
&emsp;&emsp;可以看到，首先对添加位置进行了合法检查，之后，若现有List长度不够，调用grow()方法对原有List容量加一，之后调用arraycopy方法对插入位置之后的元素进行复制。最后将待插入元素插入应插入空位，List的size属性加一。

<br>
</br>

### 2.2 删除

 - remove(int index)方法
&emsp;remove(int index)方法的结果是删除指定位置的元素。源码如图2-4所示：
 <center>
<img src="https://img-blog.csdnimg.cn/20200229203912707.png" width="70%" height="25%">
</center>
<center>
图2-4 remove(int index)方法
</center>
&emsp;&emsp;remove()方法首先检查删除位置是否符合删除要求，调用fastRemove()方法进行元素的删除。fastRemove()方法源码如图2-5所示：
<center>
<img src = "https://img-blog.csdnimg.cn/20200229204526295.png" width="70%" height="25%">
</center>
<center>
图2-5 fastReove()方法
</center>
&emsp;&emsp;即如果size-1大于将删除的位置，将待删除位置之后所有元素向前移动，如果删除后List为空，返回null。

<br>
</br>

 - remove(Object object)方法
&emsp;&emsp;remove(Object object)的结果是删除某个已知元素，源码如图2-6所示：
 <center>
<img src="https://img-blog.csdnimg.cn/20200229211409697.png" width="40%" height="25%">
</center>
<center>
图2-6 remove(Object object)方法
</center>
&emsp;&emsp;通过循环遍历，并进行比较找到对应的index。

&emsp;&emsp;这里有两点需要注意：
	&emsp;&emsp;&emsp;1. 传入null情况：
	&emsp;&emsp;&emsp;&emsp;删除掉第一个null元素。
&emsp;&emsp;&emsp;2. 顺序删除问题：
	&emsp;&emsp;&emsp;&emsp;ArrayList是可以顺序删除节点的，如果使用普通for循环，必须是从后往前删。
	示例代码如下：
```java
	public static void main(String[] args) {
	       ArrayList list = new ArrayList();
	       list.add("a");
	       list.add("b");
	       list.add("c");
	   
	       System.out.println("删除前：" + list.toString());
	       
	       for (int i = 0; i < list.size(); i++){
	           list.remove(i);
	       }
	       System.out.println("删除后：" + list.toString());    
	    } 
    }
```
&emsp;&emsp;输出结果如图2-7所示：
<center>
<img src="https://img-blog.csdnimg.cn/20200229212840567.png" width="40%" height="25%">
</center>
<center>
图2-7 错误结果
</center>
&emsp;&emsp;出错原因分析如下：

&emsp;&emsp;&emsp;要顺序删除ArrayList的全部节点，如果我们从前往后的顺序删除，先删除【0】位置的数据，但是由于删除的时候是从后往前挪一位进行删除的，所以【0】的位置又会被下一个位置的数据覆盖上，实际上【0】还是有数据的。
	
<br>
</br>

&emsp;&emsp;正确代码如下：

```java
    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        list.add("a");
        list.add("b");
        list.add("c");

        System.out.println("删除前：" + list.toString());

        for (int i = list.size() - 1; i >= 0; i--) {
            list.remove(i);
        }
        System.out.println("删除后：" + list.toString());
    }
}
```

&emsp;&emsp;输出结果如2-8所示：
<center>
<img src="https://img-blog.csdnimg.cn/20200229213611700.png" width="40%" height="25%">
</center>
<center>
图2-7 正确结果
</center>

&emsp;&emsp;所以，如果自定义类想要使用remove方法从列表删除某个指定值对象， 还需要实现该类型自己的equals方法才行！

<br>
</br>
&emsp;&emsp;当然了，清除List中所有元素可以调用clear()方法。

<br>
</br>
<br>
</br>

### 2.3 查改
 - get(int index)方法
&emsp;get(int index)方法的结果是查找指定位置的元素。源码如图2-8所示：
 <center>
<img src="https://img-blog.csdnimg.cn/20200301211054522.png" width="50%" height="25%">
</center>
<center>
图2-8 get(int index)方法
</center>
&emsp;&emsp;get()方法首先检查查找位置是否符合要求，之后调用elementData方法返回相应位置的元素，elementData()方法源码如图2-9所示：
<center>
<img src = "https://img-blog.csdnimg.cn/20200301211354504.png" width="50%" height="25%">
</center>
<center>
图2-9 elementData(int index)方法
</center>

<br>
</br>

 - set(int index, E element)方法
&emsp;set(int index, E element)方法的结果是将index位置元素设置为element元素。源码如图2-10所示：
 <center>
<img src="https://img-blog.csdnimg.cn/20200301211908437.png" width="60%" height="25%">
</center>
<center>
图2-10 set(int index, E element)方法
</center>
&emsp;&emsp;set(int index, E element)方法首先检查修改位置是否符合要求，之后取出修改之前的元素，修改相应位置元素，返回修改前的元素。这里说明一下，返回元素可以不接收，仅是调用修改。

<br>
</br>

<br>
</br>



## 第三节 ArrayList其他常用方法

 - size()方法
 &emsp;&emsp;顾名思义，求取List长度，源码短的不好意思放出来了，就是直接将size返回。
 - isEmpty()方法
 &emsp;&emsp;依旧顾名思义，判断List是否为空。
 - indexOf(Object o)方法
 &emsp;&emsp;返回查找对象o第一次出现在List中的下标，如图3-1所示，可以看到，分为了对象是否为null进行顺序查找。
<center>
<img src="https://img-blog.csdnimg.cn/20200301214123354.png" width="70%" height="25%">
</center>
<center>
图3-1 indexOf(Object o)方法
</center>

 - contains(Object o)方法
 &emsp;&emsp;判断List中是否有对象o，返回值为boolean类型。

<br>
</br>

这里就介绍这么多，更多的方法可以查看Java API规范，这里的链接是JDK 13版本的   [Java API规范](https://docs.oracle.com/en/java/javase/13/docs/api/index.html)，或者直接在调用某方法的方法名上，Ctrl + 左键，跳转到源码的位置。


