---
title: 朴素贝叶斯法
draft: false
math: true
date: 2020-09-05 22:01:42
cover: images/featureimages/8.jpg
summary: 感知机学习模型
tags: [算法,机器学习]
categories: [机器学习]
---

## 一、基本形式

&emsp;&emsp;朴素贝叶斯法(*naive&ensp;Bayes*)是基于贝叶斯定理与特征条件独立假设的分类方法。
&emsp;&emsp;对于给定的训练模型，首先基于特征条件独立假设学习输入输出的联合概率分布；然后基于此模型，对于给定的输入 $x$，利用贝叶斯定理求出后验概率最大的输出 $y$。

### 1.1 朴素贝叶斯法的基本方法

&emsp;&emsp;设输入空间 $\mathcal{X}\in \textbf{R}^n$ 为 $n$ 维向量的集合，输出空间为类标记集合 $\mathcal{Y}=\left\{c_1,c_2,\cdots,c_K\right\}$。输入为特征向量 $x\in \mathcal{X}$，输出为类标记(class&ensp;label) $y\in \mathcal{Y}$。$X$ 是定义在输入空间 $\mathcal{X}$ 上的特征向量，$Y$ 是定义在输出空间 $\mathcal{Y}$ 上的随机变量。$P\left(X,Y\right)$ 是 $X$ 和 $Y$ 的联合概率分布。训练数据集 $T=\left\{\left(x_1,y_1\right),\left(x_2,y_2\right),\cdots ,\left(x_N,y_N\right)\right\}$ 由 $P\left(X,Y\right)$ 独立同分布产生。

&emsp;&emsp;由于我们的目标是计算 $P\left(Y|X\right)$，又贝叶斯定理可知，
$$
P\left(Y|X\right)=\cfrac{P\left(X,Y\right)}{P\left(X\right)}=\cfrac{P\left(X|Y\right)\cdot P\left(Y\right)}{\sum_{k}{P\left(X=x_i|Y\right)P\left(Y\right)}}
$$
由于分母的 $P\left(X\right)$ 是常数，所以只需要计算分子即可，即
$$
P\left(Y=c_k\right),\  k=1,2,\cdots,K
$$
$$
P\left(X=x|Y=c_k\right)=P\left(X^{\left(1\right)}=x^{\left(1\right)},\cdots,X^{\left(n\right)}=x^{\left(n\right)}|Y=c_k\right), \ k=1,2,\cdots,K
$$
但是由于条件概率分布 $P\left(X=x|Y=c_k\right)$ 具有指数级数量的参数，其估计实际上是不可行的。因此，朴素贝叶斯对条件概率分布做了**条件独立性**的假设，由于这是一个较强的假设，由是也叫作“朴素”。
$$
\begin{aligned}
	P\left( X=x\left| Y=c_k \right. \right) &=P\left( X^{\left( 1 \right)}=x^{\left( 1 \right)},\cdots ,X^{\left( n \right)}=x^{\left( n \right)}\left| Y=c_k \right. \right)\\\\
	&= \prod_{j=1}^n{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}\\
\end{aligned}
$$

&emsp;&emsp;朴素贝叶斯法学习的是生成数据的机制，因此是一种学习模型。条件独立假设即用于分类的特征在类确定的条件下都是条件独立的，这个假设使得整个算法变得简单，但是有时也会牺牲一些准确率。

&emsp;&emsp;因此，朴素贝叶斯的基本公式为
$$
P\left( Y=c_k\left| X=x \right. \right) =\frac{P\left( Y=c_k \right) \prod_j{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}}{\sum_k{P\left( Y=c_k \right) \prod_j{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}}},\ k=1,2\cdots ,K
$$
朴素贝叶斯分类器可以表示为
$$
y=f\left(x\right)=\arg \underset{c_k}{\max}\frac{P\left( Y=c_k \right) \prod_j{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}}{\sum_k{P\left( Y=c_k \right) \prod_j{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}}}
$$
注意到分母对所有的 $c_k$ 都是相同的，所以
$$
y=f\left(x\right)=\arg \underset{c_k}{\max}P\left( Y=c_k \right) \prod_j{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}
$$

### 1.2 后验概率最大化的含义

朴素贝叶斯法将实例分到后验概率最大的类中，等价于期望风险最小化。我们假设一个 $0-1$ 损失函数：
$$
L\left(Y,f\left(X\right)\right)=\left\{ 
    \begin{array}{l}
	1,\ Y\ne f\left( X \right)\\\\
	0,\ Y=f\left( X \right)
\end{array} \right .
$$
式中 $f\left(X\right)$是分类决策函数，此时的期望风险函数为
$$
R_{\exp}\left(f\right)=E\left[L\left(Y,f\left(X\right)\right)\right]
$$
此为对联合概率分布 $P\left(X,Y\right)$ 求取的期望，由此，取条件期望
$$
R_{\exp}\left( f \right) =E_X\sum_{k=1}^K{\left[ L\left( c_k,f\left( X \right) \right) \right] P\left( c_k|X \right)}
$$
为了使期望风险最小化，只需对 $X=x$ 逐个最小化，由此得到：
$$
\begin{aligned}
f\left(x\right) &=\arg \underset{y\in \mathcal{Y}}{\min}\sum_{k=1}^K{L\left(c_k,y\right)P\left(c_k|X=x\right)}\\\\
&=\arg \underset{y\in \mathcal{Y}}{\min}\sum_{k=1}^K{P\left(y\ne c_k|X=x\right)}\\\\
&=\arg \underset{y\in \mathcal{Y}}{\min}\left(1-P\left(y=c_k|X=x\right)\right)\\\\
&=\arg \underset{y\in \mathcal{Y}}{\max}P\left(y=c_k|X=x\right)\\
\end{aligned}
$$
由此，根据期望风险最小化准则就得到了后验概率最大化准则：
$$
f\left(x\right)=\arg \underset{c_k}{\max}P\left(c_k|X=x\right)
$$
这也是朴素贝叶斯采用的定理。

## 二、朴素贝叶斯法的参数估计

### 2.1 极大似然估计

&emsp;&emsp;在朴素贝叶斯法中，需要知道先验概率分布 $P\left(Y=c_k\right)$ 与条件概率分布 $P\left(X^{\left(j\right)}=x^{\left(j\right)}|Y=c_k\right)$ 由于我们是用训练数据集去预测，因此需要用训练数据集估计新输入的实例，可以应用极大似然估计去估计相应的概率。
&emsp;&emsp;先验概率 $P\left(Y=c_k\right)$ 的极大似然估计为
$$
P\left(Y=c_k\right)=\cfrac{\sum_{i=1}^N{I\left(y_i=c_k\right)}}{N} , k=1,2,\cdots,K
$$
设第 $j$ 个特征 $x^{\left(j\right)}$ 的可能取值集合为 $\left\{a_{j1},a_{j2},\cdots,a_{jS_j}\right\}$，条件概率 $P\left(X^{\left(j\right)}=a_{jl}|Y=c_k\right)$ 的极大似然估计为
$$
\begin{array}{c}
	P\left( X^{\left( j \right)}=a_{jl}\left| Y=c_k \right. \right) =\frac{\sum_{i=1}^N{I\left( x_{i}^{\left( j \right)}=a_{jl},y_i=c_k \right)}}{\sum_{i=1}^N{I\left( y_i=c_k \right)}}\\\\
    \\\\
	j=1,2,\cdots ,n;\ l=1,2,\cdots ,S_j;\ k=1,2,\cdots ,K\\
\end{array}
$$
式中，$x_{i}^{\left( j \right)}$ 是第 $i$ 个样本的第 $j$ 个特征；$a_{jl}$ 是第 $j$ 个特征可能取的第 $l$ 个值；$I$ 为指示函数。

### 2.2 学习与分类算法

&emsp;&emsp;<font size = 4>__算法 1（朴素贝叶斯算法）__</font>

&emsp;&emsp;输入：训练数据集 $T=\left\{\left(x_1,y_1\right),\left(x_2,y_2\right),\cdots,\left(x_N,y_N\right)\right\}$ ，其中，$x_i=\left( x_{i}^{\left( 1 \right)},x_{i}^{\left( 2 \right)},\cdots ,x_{i}^{\left( n \right)} \right) ^{\text{T}}$，$x_i^{\left(j\right)}$ 是第 $i$ 个样本的第 $j$ 个特征，$x_i^{\left(j\right)}\in \left\{a_{j1},a_{j2},\cdots,a_{jS_j}\right\}$，$a_{jl}$ 是第 $j$ 个特征可能取的第 $l$ 值，$j=1,2,\cdots,n$，$l=1,2,\cdots,S_j$，$y_i\in \left\{c_1,c_2,\cdots,c_K\right\}$；实例 $x$；

&emsp;&emsp;输出：实例 $x$ 的分类。

1. 计算先验概率及条件概率;
$$
P\left(Y=c_k\right)=\cfrac{\sum_{i=1}^N{I\left(y_i=c_k\right)}}{N} , k=1,2,\cdots,K
$$
$$
\begin{array}{c}
	P\left( X^{\left( j \right)}=a_{jl}\left| Y=c_k \right. \right) =\frac{\sum_{i=1}^N{I\left( x_{i}^{\left( j \right)}=a_{jl},y_i=c_k \right)}}{\sum_{i=1}^N{I\left( y_i=c_k \right)}}\\\\
    \\\\
	j=1,2,\cdots ,n;\ l=1,2,\cdots ,S_j;\ k=1,2,\cdots ,K\\
\end{array}
$$

2. 对于给定的实例 $x_i=\left( x_{i}^{\left( 1 \right)},x_{i}^{\left( 2 \right)},\cdots ,x_{i}^{\left( n \right)} \right) ^{\text{T}}$，计算

$$
P\left( Y=c_k \right) \prod_{j=1}^n{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}, k=1,2,\cdots,K
$$

3. 确定实例 $x$ 的类

$$
y=f\left(x\right)=\arg \underset{c_k}{\max}P\left( Y=c_k \right) \prod_{j=1}^n{P\left( X^{\left( j \right)}=x^{\left( j \right)}\left| Y=c_k \right. \right)}
$$

### 2.3 贝叶斯估计

&emsp;&emsp;用极大似然法估计可能会出现待估计的概率值为0的情况，于是乎一系列的计算全为0了，会使的分类的结果产生偏差。采取的办法是，在所有情况的频数上加上统一的数，既保证了每项概率不为0，同时所有项的概率和也为1。这就是条件概率的贝叶斯估计。
$$
P_{\lambda}\left( X^{\left( j \right)}=a_{jl}\left| Y=c_k \right. \right) =\frac{\sum_{i=1}^N{I\left( x_{i}^{\left( j \right)}=a_{jl},y_i=c_k \right)}+\lambda}{\sum_{i=1}^N{I\left( y_i=c_k \right)}+S_j\lambda}
$$
式中 $\lambda\ge1$，当 $\lambda=0$ 时就是极大似然估计，当 $\lambda=1$ 时，称为拉普拉斯平滑(*Laplacian&ensp;smoothing*)。显然，对 $\forall l=1,2,\cdots ,S_j$，$k=1,2,\cdots,K$，有
$$
P_{\lambda}\left( X^{\left( j \right)}=a_{jl}\left| Y=c_k \right. \right) >0
$$
$$
\sum_{l=1}^{S_j}{P_{\lambda}\left( X^{\left( j \right)}=a_{jl}\left| Y=c_k \right. \right)}=1
$$
同理，先验概率的贝叶斯估计为
$$
P\left(Y=c_k\right)=\cfrac{\sum_{i=1}^N{I\left(y_i=c_k\right)+\lambda}}{{N+K\lambda}}
$$

Python代码实现如下：

```python
def NaiveBayes(Py, Px_y, x):
    """
    通过朴素贝叶斯进行概率估计
    :param Py: 先验概率分布
    :param Px_y: 条件概率分布
    :param x: 要估计的样本x
    :return: 返回所有label的估计概率
    """
    # 设置特征数目
    featrueNum = AAA
    # 设置类别数目
    classNum = BBB
    # 建立存放所有标记的估计概率数组
    P = [0] * classNum
    # 对于每一个类别，单独估计其概率
    for i in range(classNum):
        # 初始化sum为0，sum为求和项。
        # 在训练过程中对概率进行了log处理，所以这里原先应当是连乘所有概率，最后比较哪个概率最大
        # 但是当使用log处理时，连乘变成了累加，所以使用sum
        sum = 0
        # 获取每一个条件概率值，进行累加
        for j in range(featrueNum):
            sum += Px_y[i][j][x[j]]
        # 最后再和先验概率相加
        P[i] = sum + Py[i]

    # max(P)：找到概率最大值
    # P.index(max(P))：找到该概率最大值对应的所有（索引值和标签值相等）
    return P.index(max(P))


def getAllProbability(trainDataArr, trainLabelArr):
    """
    通过训练集计算先验概率分布和条件概率分布
    :param trainDataArr: 训练数据集
    :param trainLabelArr: 训练标记集
    :return: 先验概率分布和条件概率分布
    """
    # AAA与BBB根据实际需要修改
    featureNum = AAA
    classNum = BBB

    # 初始化先验概率分布存放数组，后续计算得到的P(Y = 0)放在Py[0]中，以此类推
    # 数据长度为BBB行1列
    Py = np.zeros((classNum, 1))
    # 对每个类别进行一次循环，分别计算它们的先验概率分布

    for i in range(classNum):
        # 下方式子拆开分析
        # np.mat(trainLabelArr) == i：将标签转换为矩阵形式，里面的每一位与i比较，若相等，该位变为Ture，反之False
        # np.sum(np.mat(trainLabelArr) == i):计算上一步得到的矩阵中Ture的个数，进行求和(直观上就是找所有label中有多少个为i的标记，求得4.8式P（Y = Ck）中的分子)
        # np.sum(np.mat(trainLabelArr) == i)) + 1
        # (len(trainLabelArr) + 10)：标签集的总长度+10.
        # ((np.sum(np.mat(trainLabelArr) == i)) + 1) / (len(trainLabelArr) + 10)：最后求得的先验概率
        Py[i] = ((np.sum(np.mat(trainLabelArr) == i)) + 1) / (len(trainLabelArr) + 10)
    # 转换为log对数形式
    # log书中没有写到，但是实际中需要考虑到，原因是这样：
    # 最后求后验概率估计的时候，形式是各项的相乘
    # 1.某一项为0时，结果为0.
    # 这个问题通过分子和分母加上一个相应的数可以排除，使用2.3节中提到的拉普拉斯平滑。
    # 2.如果特征特别多，加一个先验概率分布一共很多项相乘，所有数都是0-1之间，结果一定是一个很小的接近0的数。理论上可以通过结果的大小值判断， 但在
    # 程序运行中很可能会向下溢出无法比较，因为值太小了。所以人为把值进行log处理。此外连乘项通过log以后，可以变成各项累加，简化了计算。
    # 在似然函数中通常会使用log的方式进行处理
    Py = np.log(Py)

    # 计算条件概率 Px_y=P（X=x|Y = y）
    # 计算条件概率分成了两个步骤，下方第一个大for循环用于累加，下方第一个大for循环内部是用于计算的分子，至于分子的+1以及分母的计算在下方第二个大For内
    # 初始化为全0矩阵，用于存放所有情况下的条件概率
    Px_y = np.zeros((classNum, featureNum, 2))
    # 对标记集进行遍历
    for i in range(len(trainLabelArr)):
        # 获取当前循环所使用的标记
        label = trainLabelArr[i]
        # 获取当前要处理的样本
        x = trainDataArr[i]
        # 对该样本的每一维特诊进行遍历
        for j in range(featureNum):
            # 在矩阵中对应位置加1
            # 这里还没有计算条件概率，先把所有数累加，全加完以后，在后续步骤中再求对应的条件概率
            Px_y[label][j][x[j]] += 1

    # 第二个大for，计算分母，以及分子和分母之间的除法
    # 循环每一个标记（共10个）
    for label in range(classNum):
        # 循环每一个标记对应的每一个特征
        for j in range(featureNum):
            # 获取y=label，第j个特诊为0的个数
            Px_y0 = Px_y[label][j][0]
            # 获取y=label，第j个特诊为1的个数
            Px_y1 = Px_y[label][j][1]
            # 对分子和分母进行相除，再除之前依据贝叶斯估计，分母需要加上每个特征可取值个数
            # 分别计算对于y = label，x第j个特征为0和1的条件概率分布
            Px_y[label][j][0] = np.log((Px_y0 + 1) / (Px_y0 + Px_y1 + 2))
            Px_y[label][j][1] = np.log((Px_y1 + 1) / (Px_y0 + Px_y1 + 2))

    # 返回先验概率分布和条件概率分布
    return Py, Px_y

```
