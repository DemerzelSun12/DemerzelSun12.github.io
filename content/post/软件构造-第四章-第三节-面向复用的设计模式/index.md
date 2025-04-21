---
title: 软件构造-第四章-第三节-面向复用的设计模式
date: 2020-04-20 13:34:12
draft: false
math: true
cover: images/featureimages/10.jpg
summary: 软件构造课程笔记
tags: [软件构造,Java]
categories: ["软件构造"]
---

软件构造第四章 第三节 面向复用的设计模式
 ================
<!-- toc -->

 <!--more-->

# 一、结构型模式：Structural patterns

## 1.1 适配器模式(Adapter)

1. 目的：将某个类/接口转换为用户期望的其他形式。
2. 含义：适配器模式是作为两个互不相容的接口的桥梁，将某个类/接口转换为client期望的其他形式。适配器模式使得原本由于接口不兼容而不能一起工作的的那些类/接口可以一起工作。
3. 用途：主要解决在软件系统中，需要将现存的类放到新的环境中，而环境要求的接口是现对象不能满足的。
4. 实现方法：通过增加一个新的接口，将已存在的子类封装起来，client直接面向接口编程，从来隐藏了具体子类。适配器继承或依赖已有的对象，实现想要的目标接口。
5. 对象：将旧组件重用到新系统（也称为“包装器”）。
6. 模型：

<center>
<img src="https://img-blog.csdnimg.cn/20200428232000619.png" width="80%" height="35%">
</center>
<center>
图1-1 适配器模式模型
</center>

7. 例：问题描述：
   LegacyRectangle是已有的类（需要传入矩形的一个顶点、长和宽），但是与client要求的接口不一致（需要给出对角线两个顶点坐标），我们此时建立一个新的接口Shape以供客户端调用，用户通过Shape接口传入两个点的坐标。Rectangle作为Adapter，实现该抽象接口，通过具体的方法实现适配。
在不适用适配器时：会发生委派不相容。

<center>
<img src="https://img-blog.csdnimg.cn/20200506150844752.png" width="80%" height="35%">
</center>
<center>
图1-2 LegacyRectangle类与Client类
</center>

<center>
<img src="https://img-blog.csdnimg.cn/20200506151156221.png" width="80%" height="35%">
</center>
<center>
图1-3 Shape接口
</center>

## 1.2 装饰器模式(Decorator)

1. 含义：
   - 装饰器模式允许向一个现有的对象添加新的功能，同时又不改变其结构，它是作为一个现有的类的一个包装。
   - 这种模式创建了一个装饰类，用来包装原有的类，并在保证类方法签名完整性的前提下，提供额外的功能。
   - 装饰模式是继承的一个代替模式，装饰模式可以动态地给一个对象添加一些额外的职能。就增加功能来说，装饰器模式比生成子类更为灵活。
2. 主要解决：
   - 一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。
3. 实现方法：
   - 将具体功能职责划分，对每一个特性构造子类，通过委派机制增加到对象上。
4. 用途：
   - 为对象增加不同侧面的特性。
5. 优点：
   - 装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。
6. 缺点：
   - 多层装饰比较复杂，客户端需要一个具有多特性的object，需要一层一层的装饰来实现
7. 模型：
<center>
<img src="https://img-blog.csdnimg.cn/20200506153427110.jpg" width="80%" height="35%">
</center>
<center>
图1-4 装饰器模式模型
</center>

8. 例：
   - 需要对Stack数据结构的各种扩展：
     - UndoStack：一个允许你撤销先前的push或pop操作的栈
     - SecureStack：一个需要密码的栈
     - SynchronizedStack：一个串行化并发访问的栈
   - 可以采用继承的方式来解决。之后我们又需要任意可组合的扩展：
     - SecureUndoStack：需要密码的堆栈，并且还可以撤消以前的操作
     - SynchronizedUndoStack：一个堆栈，用于序列化并发访问，还可以让您撤销先前的操作
     - SecureSynchronizedStack：需要密码的堆栈，并用于序列化并发访问的操作
   - 装饰器模式就可以很好地解决这一问题。

<center>
<img src="https://img-blog.csdnimg.cn/20200506163409391.png" width="80%" height="35%">
</center>
<center>
图1-5 Stack的基础类
</center>

<center>
<img src="https://img-blog.csdnimg.cn/20200506163559852.png" width="80%" height="35%">
</center>
<center>
图1-6 UndoStack拓展
</center>

9. 实现方式：
   - 通过包装器的层层装饰实现

<center>
<img src="https://img-blog.csdnimg.cn/20200506163805538.png" width="90%" height="35%">
</center>
<center>
图1-7 Stack具体声明
</center>

## 1.3 外观模式(facade)

1. 含义：
   - 外观模式隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口。这种类型的设计模式属于结构型模式，它向现有的系统添加一个接口，来隐藏系统的复杂性。这种模式涉及到一个单一的类，该类提供了客户端请求的简化方法和对现有系统类方法的委托调用。
2. 意图：
   - 为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。
3. 主要解决：
   - 降低访问复杂系统的内部子系统时的复杂度，简化客户端与之的接口。
4. 实现方法：
   - 提供一个统一的接口来取代一系列小接口调用，相当于对复杂系统做了一个封装，简化客户端使用。便于客户端学习，解耦 。
5. 用途：
   - 为了解决客户端需要通过一个简化的接口来访问复杂系统内的功能这一问题提出的。
6. 优点：
   - 减少系统相互依赖
   - 提高灵活性
   - 提高了安全性
7. 缺点：
   - 不符合开闭原则，如果要改东西很麻烦，继承重写都不合适。
8. 模式：

<center>
<img src="https://img-blog.csdnimg.cn/20200506164429965.png" width="90%" height="35%">
</center>
<center>
图1-8 外观模式模型
</center>

9. 例：
   - 假设我们有一个具有一组接口的应用程序来使用MySql / Oracle数据库，并生成不同类型的报告，如HTML报告，PDF报告等。
   - 因此，我们将有不同的接口集合来处理不同类型的数据库。现在客户端应用程序可以使用这些接口来获取所需的数据库连接并生成报告。但是，当复杂性增加或界面行为名称混淆时，客户端应用程序将难以管理它。
   - 因此，我们可以在这里应用Facade模式，并在现有界面的顶部提供包装界面以帮助客户端应用程序。

<center>
<img src="https://img-blog.csdnimg.cn/20200506165520949.png" width="90%" height="35%">
</center>
<center>
图1-9 Two Helper Classes for MySQL and Oracle： 分别封装了客户端所需的功能
</center>

<center>
<img src="https://img-blog.csdnimg.cn/20200506165610574.png" width="90%" height="35%">
</center>
<center>
图1-10 外观模式的类
</center>

<center>
<img src="https://img-blog.csdnimg.cn/20200506165617187.png" width="90%" height="35%">
</center>
<center>
图1-11 客户端代码
</center>

# 二、行为类模式：Behavioral patterns

## 2.1 策略模式(Strategy)

1. 含义：
   - 在策略模式中，一个类的行为或算法可以在运行时更改。在策略模式中，我们创建表示各种模式的对象和一个行为随着策略对象改变而改变的 context对象，策略对象改变 context对象的执行算法。
2. 用途：
   - 针对特定任务存在不同的算法，但客户端可以根据动态上下文在运行时切换算法。  
3. 实现方法：
   - 为算法创建一个接口，并为算法的每个变体创建一个实现类。
4. 解决方法：
   - 将这些算法封装成一个一个的类，任意地替换
5. 优点：
   - 算法可以自由切换
   - 避免使用多重条件判断
   - 扩展性良好
6. 缺点：
   - 策略类会增多
   - 所有策略类都需要对外暴露。
7. 模型：
<center>
<img src="https://img-blog.csdnimg.cn/2020050617163393.png" width="90%" height="35%">
</center>
<center>
图2-1 策略模式模型
</center>

8. 例：
[策略模式示例](https://www.runoob.com/design-pattern/strategy-pattern.html)

## 2.2 模板模式(Template method)

1. 含义：
   - 在模板模式中，一个抽象类公开定义了执行它的方法的方式/模式，它的子类可以按照需要重写实现方法，但调用将以抽象类中定义的方法进行。
2. 意图：
   - 定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
3. 实现方法：
   - 共性的步骤在抽象类内公共实现，差异化的步骤在各个子类中实现。一般使用继承和重写实现模板模式。
4. 优点：
   - 封装不变部分，扩展可变部分
   - 提取公共代码，便于维护
   - 行为由父类控制吗，子类实现
5. 缺点：
   - 每一个不同的实现都需要一个子类来实现
   - 导致类的个数增加，使得系统更加庞大
6. 模式：

<center>
<img src="https://img-blog.csdnimg.cn/20200506172804965.png" width="90%" height="35%">
</center>
<center>
图2-2 模板模式模型
</center>

7. 例：
[模板模式示例](https://www.runoob.com/design-pattern/template-pattern.html)

## 2.3 迭代器模式(Iterator)

1. 含义：
   - 这种模式用于顺序访问集合对象的元素，而又无需暴露该对象的内部表示。
2. 用途：
   - 解决 客户需要统一的策略来访问容器中的所有元素，与容器类型无关
3. 实现方法：
   - 这种模式让自己的集合类实现Iterable接口，并实现自己的独特Iterator迭代器(hasNext, next, remove），允许客户端利用这 个迭代器进行显式或隐式的迭代遍历。
4. 优点：
   - 它支持以不同的方式遍历一个聚合对象
   - 迭代器简化了聚合类
   - 在同一个聚合上可以有多个遍历
   - 在迭代器模式中，增加新的聚合类和迭代器类都很方便，无须修改原有代码
5. 缺点：
   - 由于迭代器模式将存储数据和遍历数据的职责分离，增加新的聚合类需要对应增加新的迭代器类，类的个数成对增加，这在一定程度上增加了系统的复杂性。
6. 模式：

<center>
<img src="https://img-blog.csdnimg.cn/20200506173438304.png" width="90%" height="35%">
</center>
<center>
图2-3 迭代器模式模型
</center>

7. 例：
[迭代器模式示例](https://www.runoob.com/design-pattern/iterator-pattern.html)
