---
title: 软件构造第一章总结
date: 2020-03-08 15:27:38
draft: false
math: true
cover: images/featureimages/0.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

 软件构造第一章总结
 ================
<!-- toc -->

## 一、软件构造多维度视图

### 1.1 从三个维度看软件系统的构成

1. 按阶段划分：build-time（构造阶段）和run-time（运行阶段）
2. 按动态划分：moment（时刻）和period（时期）
3. 按层次划分：code（代码层面）和component（组件，文件层面）
<center>
<img src="https://img-blog.csdnimg.cn/20200313184621493.jpg" width="70%" height="25%">
</center>
<center>
图1-1 软件构造多维度视图
</center>

 <!--more-->


## 二、软件构造的阶段划分、各阶段的构造活动

### 2.1. Build-time

&emsp;&emsp;想法⇒\Rightarrow⇒需求⇒\Rightarrow⇒设计⇒\Rightarrow⇒代码⇒\Rightarrow⇒可安装并执行的包
&emsp;&emsp;Code-level view：代码的逻辑组织（函数、类、方法、接口······）
&emsp;&emsp;Component-level view：代码的物理组织（文件、目录、包、程序库······）
&emsp;&emsp;Moment view：特定时刻的软件形态
&emsp;&emsp;Period view：软件形态随时间的变化

**1. build-time; moment; code-level**
三种相互关联的形式

面向词法：半结构化源代码
面向词法：（AST抽象语法树）半结构化的源代码变成语法树（编译器能够处理）
面向词法：UML视图（通常是图形化或形式化的）
<center>
<img src="https://img-blog.csdnimg.cn/20200313193130236.png" width="70%" height="25%">
</center>
<center>
图2-1 UML图
</center>


**2. build-time; period; code-level**
Code churn（代码变化）

**3. build-time; moment; component-level**
源代码如何组织成文件——通过类库
文件被压缩进package，逻辑上进入components（组件）and sub-systems（子系统）

**链接技术（动态 / 静态）**
Library

1. 操作系统提供的库
2. 编程语言提供的库
3. 第三方公司提供的库
4. 自己编写的库

链接：编程时和build时，需告诉IDE和JVM在哪里寻找某些库。

静态链接

库被拷贝进入代码形成整体，执行的时候无需提供库文件。
静态链接发生在构造阶段。

**4. build-time; period; component-level**
SCI 配置项的更改
VCS 版本控制系统
各项软件实体随时间如何变化
软件随时间变化的版本
版本控制（通过Git，SVN等等）

### 2.2 Run-time

程序被载入目标机器，开始执行。

Code-level view：逻辑实体在内存中的呈现方式
Component-level view：物理实体在物理硬件环境中的呈现方式

Moment view：在内存/硬件环境中特定时刻的形态
Period view：硬件环境中的形态随时间的变化

动态链接

库文件不会在build阶段被加入可执行软件，仅仅做出标记。
程序运行时，根据标记装载库至内存。
发布软件时，记得将程序所依赖的所有动态库都复制给用户。
分布式程序

需要多个运行程序，分别部署于多个计算机物理环境。
需要多端口或者多线程。

**5. run-time; moment; code-level**
Code SnapShot（代码快照）
描述程序运行时内存里变量层面的状态，即某一时间点内存中对象及对象的值。

Memory dump（内存信息转储）
常发生在异常退出时，把内存中信息写到文件中（常用来调试）。

**6. run-time, period and code-level view**
UML运行时图，描述程序运行时间各个种类间的依赖关系。
Execution tracing（执行跟踪）
用日志方式记录程序执行的调用次序。

**7. run-time; moment; component-level**
UML部署图

执行跟踪：根据跟踪日志里的信息进行调试或诊断软件问题。

**8. run-time; period; component-level**
事件日志（系统层面）

| Event logging | Software tracing |
| ------ | ------ |
| Consumed primarily by system administrators | Consumed primarily by developers |
| Logs "high level" information (e.g. failed installation of a program) | Logs "low level" information (e.g. a thrown exception) |
| Must not be too "noisy" (contain many duplicate events or information not helpful to its intended audience) | Can be noisy |
| A standards-based output format is often desirable, sometimes even required | Few limitations on output format |
| Event log messages are often localized | Localization is rarely a concern |
| Addition of new types of events, as well as new event messages, need not be agile | Addition of new tracing messages must be agile |

**9. 相互联系**
<center>
<img src="https://img-blog.csdnimg.cn/20200313195306526.png
" width="70%" height="25%">
</center>
<center>
图2-2 各视图之间联系
</center>

## 三、内部/外部的质量指标

外部质量因素影响用户
内部质量因素影响软件本身和它的开发者
外部质量取决于内部质量

### 3.1 External quality factors（外部质量因素）

**1. Correctness（正确性）**
按照预先定义的“规约”执行
正确性是至高无上的质量指标
每一层保证自己的正确性，同时假设其下层是正确的
测试和调试：发现不正确、消除不正确
防御式编程：在写程序的时候就确保正确性
形式化方法：通过形式化验证发现问题

**2.Robustness（健壮性）**
针对异常情况的处理
– 健壮性是对正确性的补充
– 正确性：软件的行为要严格的符合规约中定义的行为
– 出现规约定义之外的情形的时候，软件要做出恰当的反应
出现异常时不要“崩溃”
健壮性是主观而非客观
– 未被specification覆盖的情况即为“异常情况”
– 所谓的“异常”，取决于spec的范畴

**3. Extendibility（可扩展性）**
定义：对软件的规约进行修改，是否足够容易的程度。
规模越大，扩展起来越不容易。
目的是为了变化。
两个准则（为了可扩展性）：简约主义设计，分离主义设计

**4. Reusability（可复用性）**
一次开发，多次使用
找到不同问题之间的共性

**5. Compatibility（兼容性）**
不同的软件系统之间相互可容易的集成
兼容性很重要因为开发设计不在真空中，所以需要相互联系
方法：保持设计的同构性并标准化

**6. Efficiency（性能）**
性能包括很多内容，最常见的就是时间复杂度和空间复杂度。
性能毫无意义，除非有足够的正确性。
– 对性能的关注要与其他质量属性进行折中
– 过度的优化导致软件不再适应变化和复用
过早优化是万恶之源

**7. Portability（可移植性）**
软件可方便的在不同的技术环境之间移植
硬件、操作系统中的移植

**8. Ease of use（易用性）**
容易学、安装、操作、监控
给用户提供详细的指南

**9. Functionality（功能性）**
每增加一小点功能，都确保其他质量属性不受到损失。
程序设计中一种不适宜的趋势，即软件开发者增加越来越多的功能，企图跟上竞争，其结果是程序极为复杂、不灵活、占用过多的磁盘空间。
<center>
<img src="https://img-blog.csdnimg.cn/20200313200344559.png
" width="70%" height="25%">
</center>
<center>
图3-1 功能性与其他属性的关系
</center>

**10. Timeliness（及时性）**
当用户需要的时候，要能及时地设计出来。

**11. Other qualities**
Verifiability（可验证性）
Integrity（完整性）
Repair ability（可修复性）
Economy（经济性）

### 3.2 Internal quality factors（内部质量因素）
Source code related factors such as Lines of Code (LOC), Cyclomatic Complexity（圈复杂度）, etc
Architecture-related factors such as coupling（耦合度）, cohesion（内聚度）, etc
Read ability（可读性）
Understand ability（可理解性）
Clearness（清晰性）
Size（大小）

### 3.3 Trade-off between quality properties（质量折中）
正确的软件开发过程中，开发者应该将不同质量因素之间如何做出折中的设计决策和标准明确的写下来。
虽然需要折中，但“正确性”绝不能与其他质量因素折中。
最重要的几个质量因素

Correctness and robustness: reliability（可靠性）
Extendibility and reusability: modularity（可延展性和可复用性）
OOP（面向对象编程）提高质量的方法
Correctness: encapsulation（封装）, decentralization（分权）
Robustness: encapsulation, error handling（异常处理）
Extendibility: encapsulation, information hiding
Reusability: modularity, component, models, patterns
Compatibility: standardized module and interface
Portability: information hiding, abstraction
Ease of use: GUI components, framework
Efficiency: reusable components
Timeliness: modeling, reuse
Economy: reuse
Functionality: extendibility
