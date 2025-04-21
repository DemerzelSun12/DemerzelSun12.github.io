---
title: 软件构造-第四章-第二节-面向复用的软件构造技术
date: 2020-04-16 22:24:42
draft: false
math: true
cover: images/featureimages/9.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

软件构造第四章 第二节 面向复用的软件构造技术
 ================
<!-- toc -->

 <!--more-->

# 一、设计可复用的类

## 1.1 行为子类型和LSP原则

1. 行为子类型

- 子类型多态（ Subtype polymorphism）：客户端可用统一的方式处理不同类型的对象 。
- 举例：

<center>
<img src="https://img-blog.csdnimg.cn/20200428172941869.png
" width="40%" height="35%">
</center>
<center>
图1-1 子类型多态
</center>
<center>
在可以使用a的场景，都可以用c1和c2代替而不会有任何问题。
</center>

2. 使用注意事项：
   - 在java的静态类型检查之中，编译器强调了几条规则：
     - 子类型可以增加方法，但不可删
     - 子类型需要实现抽象类型中的所有未实现方法
     - 子类型中重写的方法必须有相同或子类型的返回值
     - 子类型中重写的方法必须使用同样类型的参数
     - 子类型中重写的方法不能抛出额外的异常
   - 行为子结构也适用于指定的方法：
     - 更强的不变量
     - 更弱的前置条件
     - 更强的后置条件

3. 例：
<center>
<img src="https://img-blog.csdnimg.cn/20200428174734109.png" width="80%" height="35%">
</center>
<center>
图1-2 例一
</center>

- 子类满足相同的不变量（同时附加了一个）
- 重写的方法有相同的前置条件和后置条件
- 故该结构满足LSP

<center>
<img src="https://img-blog.csdnimg.cn/20200428174920642.png" width="80%" height="35%">
</center>
<center>
图1-3 例二
</center>

- 子类满足相同的不变量（同时附加了一个）
- 重写的方法 start 的前置条件更弱
- 重写的方法 brake 的后置条件更强
- 故该结构满足LSP

<center>
<img src="https://img-blog.csdnimg.cn/20200428191009734.png" width="80%" height="35%">
</center>
<center>
图1-4 例三
</center>

- 子类型前置条件更强
- 故该结构不满足LSP

## 1.2 协变与逆变

1. 协变与逆变综述
   - 如果A、B表示类型，f(⋅)表示类型转换，≤表示继承关系（比如，A≤B表示A是由B派生出来的子类）：
     - f(⋅)是逆变（contravariant）的，当A≤B时有f(B)≤f(A)成立；
     - f(⋅)是协变（covariant）的，当A≤B时有f(A)≤f(B)成立；
     - f(⋅)是不变（invariant）的，当A≤B时上述两个式子均不成立，即f(A)与f(B)相互之间没有继承关系。

2. 协变
   - 父类型->子类型：越来越具体(specific)。
   - 在LSP中，返回值和异常的类型：不变或变得更具体 。
   - 例：
<center>
<img src="https://img-blog.csdnimg.cn/20200428193354774.png" width="80%" height="35%">
</center>
<center>
图1-5 协变
</center>

3. 逆变(Contra-variance)：
   - 父类型->子类型：越来越抽象。
   - 参数类型：要相反的变化，不变或越来越抽象。
   - 但这在Java中是不允许的，因为它会使重载规则复杂化。
   - 例：
<center>
<img src="https://img-blog.csdnimg.cn/20200428193608834.png" width="50%" height="35%">
</center>
<center>
图1-6 逆变
</center>

4. 总结：
<center>
<img src="https://img-blog.csdnimg.cn/20200428193800233.png" width="100%" height="35%">
</center>
<center>
图1-7 协变与逆变总结
</center>

- t'是t的子类型，T'是T的子类型；
- 1.子类型（属性、方法）关系；
- 2.不变性，重写方法；
- 3.协变，方法返回值变具体；
- 4.逆变，方法参数变抽象；
- 5.协变，参数变的更具体，协变不安全

## 1.3 Liskov替换原则(LSP)

1. 里氏替换原则的主要作用就是规范继承时子类的一些书写规则。其主要目的就是保持父类方法不被覆盖。
2. 具体内容：
   - 子类必须完全实现父类的方法
   - 子类可以有自己的个性
   - 覆盖或实现父类的方法时输入参数可以被放大
   - 覆盖或实现父类的方法时输出结果可以被缩小
3. LSP是子类型关系的一个特殊定义，称为（强）行为子类型化。在编程语言中，LSP依赖于以下限制：
   - 前置条件不能强化
   - 后置条件不能弱化
   - 不变量要保持或增强
   - 子类型方法参数：逆变
   - 子类型方法的返回值：协变
   - 异常类型：协变

# 二、LSP原则的应用

## 2.1 数组是协变的

1. 数组是协变的：一个数组T[ ] ，可能包含了T类型的实例或者T的任何子类型的实例。
2. 即子类型的数组可以赋予父类型的数组进行使用，但数组的类型实际为子类型。
3. 下面报错的原因是myNumber指向的还是一个Integer[] 而不是Number[]:

<center>
<img src="https://img-blog.csdnimg.cn/20200428205811703.png" width="100%" height="35%">
</center>
<center>
图2-1 数组协变错误示例
</center>

## 2.2 泛型中的LSP

1. Java中泛型是不变的,但可以通过通配符"?"实现协变和逆变：
   - <? extends>实现了泛型的协变：
   - List<? extends Number> list = new ArrayList\<Integer>();
   - <? super>实现了泛型的逆变：
   - List<? super Number> list = new ArrayList\<Object>();
2. 由于泛型的协变只能规定类的上界，逆变只能规定下界，使用时需要遵循PECS（producer--extends, consumer-super）：
   - 要从泛型类取数据时，用extends；
   - 要往泛型类写数据时，用super；
   - 既要取又要写，就不用通配符（即extends与super都不用）。
3. 泛型是类型不变的（泛型不是协变的）。举例来说:
   - ArrayList\<String> 是List\<String>的子类型;
   - List\<String>不是List\<Object>的子类型.
4. 在代码的编译完成之后，泛型的类型信息就会被编译器擦除。因此，这些类型信息并不能在运行阶段时被获得。这一过程称之为类型擦除（type erasure）。
5. 类型擦除的详细定义：如果类型参数没有限制，则用它们的边界或Object来替换泛型类型中的所有类型参数。因此，产生的字节码只包含普通的类、接口和方法。
6. 类型擦除的结果：\<T>被擦除的T变成了Object

<center>
<img src="https://img-blog.csdnimg.cn/20200428210654226.png" width="100%" height="35%">
</center>
<center>
图2-2 泛型擦除示例
</center>

7. Integer是number的子类型，但Box\<Integer>也不是Box\<Number>的子类型，即，Box\<Integer>和Box\<Number>都为Object的子类型。

<center>
<img src="https://img-blog.csdnimg.cn/20200428211418153.png" width="100%" height="35%">
</center>
<center>
图2-3 泛型依赖示例
</center>

## 2.3 为了解决类型擦除的问题-----Wildcards（通配符）

1. 无界通配符类型使用通配符（?）指定，例如List\<?>，这被称为未知类型的列表。
2. 在两种情况下，无界通配符是一种有用的方法：
   - 如果正在编写可使用Object类中提供的功能实现的方法。
   - 当代码使用泛型类中不依赖于类型参数的方法时。 例如，List.size或List.clear。 事实上，Class <?>经常被使用，因为Class\<T>中的大多数方法不依赖于T。
3. 举例：

<center>
<img src="https://img-blog.csdnimg.cn/20200428211909459.png" width="80%" height="35%">
</center>
<center>
图2-4 无界通配符示例
</center>

- printList的目标是打印任何类型的列表，但它无法实现该目标 ，它仅打印Object实例列表; 它不能打印List\<Integer>，List\<String>，List\<Double>等，因为它们不是List \<Object>的子类型。 

- 要编写通用的printList方法，请使用List<?>
  - 低边界通配符<? super A>&emsp;&emsp; e.g. List<? super Integer>的子类型有List\<Number>
  - 上边界通配符<? extends A>&emsp;&emsp;e.g. List<? extends Number>的子类型有List\<Integer>

<center>
<img src="https://img-blog.csdnimg.cn/2020042821233595.png" width="80%" height="35%">
</center>
<center>
图2-4 低边界通配符与上边界通配符
</center>

# 三、委派与组合

## 3.1 Comparator 和 Comparable（比较器与可比较的）

1. 先看一个比较排序的例子：

<center>
<img src="https://img-blog.csdnimg.cn/2020042821265789.png" width="80%" height="35%">
</center>
<center>
图3-1 比较排序示例
</center>

- 引入 int compare(T o1 , T o2)：用于比较两个变量的大小。
- 如果设计的ADT需要比较大小，或者要放入Collections或Arrays进行排序，可实现Comparator接口并override compare()函数。下面为具体例子：

<center>
<img src="https://img-blog.csdnimg.cn/20200428213026378.jpg" width="80%" height="35%">
</center>
<center>
图3-2 重写compare()方法
</center>

- 另一种方法：让ADT实现Comparable接口，然后override compareTo() 方法。与使用Comparator的区别：不需要构建新的Comparator类，比较代码放在ADT内部。下面为具体例子。

<center>
<img src="https://img-blog.csdnimg.cn/20200428213256493.png" width="80%" height="35%">
</center>
<center>
图3-3 重写compareTo()方法
</center>

## 3.2 委托(Delegation)

1. 委托/委派：一个对象请求另一个对象的功能 。
2. 委托是复用的一种常见形式。
3. 分为显性委派：将发送对象传递给接收对象；以及隐性委派：由语言的成员查找规则。
4. 下面示例：可以看到，想在B中调用A，需要先委派一个A，若B中想执行A中的方法，委托给A执行。

<center>
<img src="https://img-blog.csdnimg.cn/2020042821351696.png" width="80%" height="35%">
</center>
<center>
图3-4 委托示例
</center>

5. 委托设计模式：是一种用来实现委派的软件设计模式；
6. 委托依赖于动态绑定，因为它要求给定的方法调用可以在运行时调用不同的代码段；
7. 委托的过程如下：Receiver对象将操作委托给Delegate对象，同时Receiver对象确保客户端不会滥用委托对象；

<center>
<img src="https://img-blog.csdnimg.cn/20200428213840673.jpg" width="80%" height="35%">
</center>
<center>
图3-5 委托执行过程
</center>

8. 例二：使用委托可以继承委托类的功能，这里list是被委托的对象，List的方法add和remove方法都可以通过list在LoggingList中被调用。

<center>
<img src="https://img-blog.csdnimg.cn/2020042821404977.png" width="80%" height="35%">
</center>
<center>
图3-6 委托示例二
</center>

## 3.3 委托与继承

1. 继承：通过新操作扩展基类或覆盖操作。
2. 委托：捕获操作并将其发送给另一个对象。
3. 许多设计模式使用继承和委派的组合。
4. Problem：如果子类只需要复用父类中的一小部分方法，Solution：可以不需要使用继承，而是通过委派机制来实现。
5. 本质上，一个类不需要继承另一个类的全部方法，通过委托机制调用部分方法。

## 3.4 复合继承原则（CRP）

1. 复合复用原则(CRP)：
   - 类应当通过它们之间的组合（通过包含其它类的实例来实现期望的功能）达到多态表现和代码复用，而不仅仅是从基础类或父类继承。
   - 我们可以将组合（Composition）理解为（has a）而继承理解为(is a)；
   - 委派可以看做Object层面的复用机制，而继承可以看做是类的层面；
2. 下面是一个关于CRP的例子：

<center>
<img src="https://img-blog.csdnimg.cn/20200428215400314.png" width="100%" height="35%">
</center>
<center>
图3-7 CRP示例
</center>

- 核心问题：每个Employee对象的奖金计算方法都不同；如果能在object层面实现一定比class层面灵活很多
- 只需针对不同子类的对象，委派能够计算该子类的奖金的方法的BonusCalculator。这样一来就不需要在子类继承的时候进行重写。

3. 总结，
   - 组合来代替继承的更普遍实现：
   - 用接口来实现系统的最基础行为
   - 接口之间用extends来实现系统功能的扩展（接口组合）
   - 类 implements 组合接口

<center>
<img src="https://img-blog.csdnimg.cn/20200428220453998.png" width="100%" height="35%">
</center>
<center>
图3-8 复合继承示例
</center>

## 3.5 委派的类型

1. 临时性委派（Dependency）：最简单的方法，调用类里的方法（use a），其中一个类使用另一个类而不实际地将其作为属性。

<center>
<img src="https://img-blog.csdnimg.cn/20200428221059422.png" width="80%" height="35%">
</center>
<center>
图3-9 临时性委派
</center>

2. 永久性委派（Association）：类之中有其它类的具体实例来作为一个变量（has a）

<center>
<img src="https://img-blog.csdnimg.cn/20200428221136421.png" width="80%" height="35%">
</center>
<center>
图3-10 永久性委派
</center>

3. 组合（Composition）：更强的委派。将一些简单的对象组合成一个更为复杂的对象，但难以变化。（is part of）

<center>
<img src="https://img-blog.csdnimg.cn/20200428221638287.png" width="80%" height="35%">
</center>
<center>
图3-11 组合
</center>

4. 聚合（Aggregation）:对象是在类的外部生成的，然后作为一个参数传入到类的内部构造器，约束更弱，可动态变化。(has a)

<center>
<img src="https://img-blog.csdnimg.cn/20200428221638304.png" width="80%" height="35%">
</center>
<center>
图3-12 聚合
</center>

## 3.6 组合与聚合

&emsp;&emsp;在组合中，当拥有的对象被破坏时，被包含的对象也被破坏。在聚合中，这不一定是真的。以生活中的事物为例：大学拥有多个部门，每个部门都有一批教授。 如果大学关闭，部门将不复存在，但这些部门的教授将继续存在。 一位教授可以在一个以上的部门工作，但一个部门不能成为多个大学的一部分。大学与部门之间的关系即为组合，而部分与教授之间的关系为聚合。

# 四、设计可复用库与框架

## 4.1 library和framework，系统层面的复用

1. 之所以library和framework被称为系统层面的复用，是因为它们不仅定义了1个可复用的接口/类，而是将某个完整系统中的所有可复用的接口/类都实现出来，并且定义了这些类之间的交互关系、调用关系，从而形成了系统整体的“架构”。

2. 相应术语：
   - API（Application Programming Interface）：库或框架的接口
   - Client（客户端）：使用API的代码
   - Plugin（插件）：客户端定制框架的代码
   - Extension Point：框架内预留的“空白”，开发者开发出符合接口要求的代码( 即plugin) ， 框架可调用，从而相当于开发者扩展了框架的功能
   - Protocol（协议）：API与客户端之间预期的交互序列。
   - Callback（反馈）：框架将调用的插件方法来访问定制的功能。
   - Lifecycle method：根据协议和插件的状态，按顺序调用的回调方法。

## 4.2 API和库

1. API是程序员最重要的资产和“荣耀”，吸引外部用户，提高声誉。 
2. 建议：始终以开发API的标准面对任何开发任务；面向“复用”编程而不是面向“应用”编程。 
3. 难度：要有足够良好的设计，一旦发布就无法再自由改变。 
4. 编写一个API需要考虑以下方面：
     - API应该做一件事，且做得很好
     - API应该尽可能小，但不能太小
     - Implementation不应该影响API
     - 记录文档很重要
     - 考虑性能后果
     - API必须与平台和平共存
     - 类的设计：尽量减少可变性，遵循LSP原则
     - 方法的设计：不要让客户做任何模块可以做的事情，及时报错

## 4.3 框架

1. 框架（Framework）是整个或部分系统的可重用设计，表现为一组抽象构件及构件实例间交互的方法;另一种定义认为，框架是可被应用开发者定制的应用骨架。前者是从应用方面而后者是从目的方面给出的定义。
2. 为了增加代码的复用性，可以使用委派和继承机制。同时，在使用这两种机制增加代码复用的过程中，我们也相应地在不同的类之间增加了关系（委派或继承关系）。而对于一个项目而言，各个不同类之间的依赖关系就可以看做为一个框架。一个大规模的项目可能由许多不同的框架组合而成。
3. 框架与设计模式：
   - 框架、设计模式这两个概念总容易被混淆，其实它们之间还是有区别的。构件通常是代码重用，而设计模式是设计重用，框架则介于两者之间，部分代码重用，部分设计重用，有时分析也可重用。在软件生产中有三种级别的重用：内部重用，即在同一应用中能公共使用的抽象块;代码重用，即将通用模块组合成库或工具集，以便在多个应用和领域都能使用；应用框架的重用，即为专用领域提供通用的或现成的基础结构，以获得最高级别的重用性。  
   - 框架与设计模式虽然相似，但却有着根本的不同。设计模式是对在某种环境中反复出现的问题以及解决该问题的方案的描述，它比框架更抽象；框架可以用代码表示，也能直接执行或复用，而对模式而言只有实例才能用代码表示;设计模式是比框架更小的元素，一个框架中往往含有一个或多个设计模式，框架总是针对某一特定应用领域，但同一模式却可适用于各种应用。可以说，框架是软件，而设计模式是软件的知识。
4. 框架分为白盒框架和黑盒框架。
   - 白盒框架：
     - 白盒框架是基于面向对象的继承机制。之所以说是白盒框架，是因为在这种框架中，父类的方法对子类而言是可见的。子类可以通过继承或重写父类的方法来实现更具体的方法。
     - 虽然层次结构比较清晰，但是这种方式也有其局限性，父类中的方法子类一定拥有，要么继承，要么重写，不可能存在子类中不存在的方法而在父类中存在。
     - 通过子类化和重写方法进行扩展（使用继承）；
     - 通用设计模式：模板方法；
     - 子类具有主要方法但对框架进行控制。
     - 允许扩展每一个非私有方法
     - 需要理解父类的实现
     - 一次只进行一次扩展
     - 通常被认为是开发者框架
   - 黑盒框架：
     - 黑盒框架时基于委派的组合方式，是不同对象之间的组合。之所以是黑盒，是因为不用去管对象中的方法是如何实现的，只需关心对象上拥有的方法。
     - 这种方式较白盒框架更为灵活，因为可以在运行时动态地传入不同对象，实现不同对象间的动态组合；而继承机制在静态编译时就已经确定好。
     - 黑盒框架与白盒框架之间可以相互转换，
     - 通过实现插件接口进行扩展（使用组合/委派）；
     - 常用设计模式：Strategy, Observer ；
     - 插件加载机制加载插件并对框架进行控制。
     - 允许在接口中对public方法扩展
     - 只需要理解接口
     - 通常提供更多的模块
     - 通常被认为是终端用户框架，平台
