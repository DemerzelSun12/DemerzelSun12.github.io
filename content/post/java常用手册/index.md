---
title: java常用手册
draft: false
math: true
date: 2022-05-30 11:30:58
cover: images/featureimages/5.jpg
summary: java常用方法
tags: [实习手册,工作手册]
categories: [实习手册]
---

## 常用数据结构

### 栈

```java
import java.util.Stack;	//引用栈

//初始化
Stack<Integer> stack = new Stack<Integer>();

//进栈
stack.push(Element);

//出栈
Element e = stack.pop();

//取栈顶值（不出栈）
Element e = stack.peek();

//判断栈是否为空
boolean flag = stack.isEmpty();

//获取栈长度
int size - stack.size();

```




