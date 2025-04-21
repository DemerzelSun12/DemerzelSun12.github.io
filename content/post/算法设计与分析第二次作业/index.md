---
title: 算法设计与分析第二次作业
draft: false
math: true
date: 2020-03-04 08:11:45
cover: images/featureimages/18.jpg
summary: 算法设计与分析课程笔记
tags: [算法设计与分析]
categories: ["算法设计与分析"]
---

# 算法设计与分析第二次作业

### 1. 证明或否证： $ f\left( n \right) +o\left(f\left( n \right) \right)=\varTheta\left(f\left( n \right) \right) $

证明：
&emsp;&emsp; 对 $ \forall c>0 $, $\exists\, n_0>0$, 对$\forall \,n\ge n_0$, 有$0\le o\left(f\left( n \right) \right)\le cf\left( n \right)$, 则$f\left( n \right)\le f\left( n \right)+o\left( f\left ( n \right) \right) \le \left( c+1 \right) f\left ( n \right) $,
&emsp;&emsp; 故有$ f\left( n \right) +o\left(f\left( n \right) \right)=\varTheta\left(f\left( n \right) \right) $.

### 2. 证明： $\varTheta\left(f\left( x \right) \right) + O \left(g\left(x\right)\right)  = O\left( max\{f\left(x\right), g\left(x\right) \}\right) $

证明：
&emsp;&emsp; 由定义可知$\exists \,c_1, c_2>0$, $\exists\, x_0$, 对$\forall \,x\ge x_0,\,c_1f\left( x \right)\le\varTheta \left (f\left(x\right)\right)\le c_2f\left( x \right), $且$\exists\, c_3 > 0$, $\exists \,x_1, $ 对$\forall\, x>x_1,$ $ 0\le O\left(g\left(x\right)\right)\le g\left(x\right). $
&emsp;&emsp;故有, 对$\forall\,x\ge max \{ x_0, \,x_1\}$, $c_1f\left(x\right)\le \varTheta\left(f\left(x\right)\right)+O\left(g\left(x\right)\right)\le c_2f\left(x\right)+cg\left(x\right)\le \left(c_2+c\right)max\{f\left(x\right)+g\left(x\right)\}$
&emsp;&emsp;故$\varTheta\left(f\left( x \right) \right) + O \left(g\left(x\right)\right)  = O\left( max\{f\left(x\right), g\left(x\right) \}\right)$

### 3. 证明或给出反例： $\varTheta\left(f\left(n\right)\right)\cap o\left(f\left(n\right)\right) = \varnothing $

证明：
&emsp;&emsp;假设$\exists\,g\left(n\right)\in\varTheta\left(f\left(n\right)\right)\cap o\left(f\left(n\right)\right)$, 则必有$g\left(n\right)\in\varTheta\left(f\left(n\right)\right)且g\left(n\right)\in o\left(f\left(n\right)\right).$
&emsp;&emsp;由定义得, 必有$\exists \,c_1,\,c_2>0,\,n_0$, $对\forall\, n\ge n_0, \,c_1f\left(n\right)\le g\left(n\right)\le c_2f\left(n\right)$, $且对\forall c>0$, $\exists n_1>0$, $\forall n\ge n_0$, 有$0\le g\left(n\right)< cf\left(n\right)$, 易知二者矛盾, 固原假设不成立.
&emsp;&emsp;即：$\varTheta\left(f\left(n\right)\right)\cap o\left(f\left(n\right)\right) = \varnothing $

### 4. 证明：设k是任意常数正整数，则$log_kn$
未完待续...大概率鸽了。。。
