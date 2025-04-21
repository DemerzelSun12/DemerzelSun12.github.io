---
title: k近邻法
draft: false
math: true
date: 2020-08-31 15:04:34
cover: images/featureimages/6.jpg
summary: 一种基本的分类与回归方法——k近邻法
tags: [算法,机器学习]
categories: [机器学习]
---


## 一、k近邻算法

&emsp;&emsp;*k*近邻法(*k-nearest&ensp;neighbor, k-NN*)是一种基本分类与回归方法，这里指的是分类问题中的*k*邻近法。*k*近邻法的输入为实例的特征向量，对应于特征空间里的点；输出为实例的类别，可以取多类。分类时，对于新的实例，根据其*k*个最邻近的训练实例的类别，通过多数表决等方式进行预测。因此，*k*邻近法不具有显式的学习过程，实际上是利用训练数据集对特征向量空间进行划分，并作为其分类的“模型”。
&emsp;&emsp;*k*近邻法的三要素：*k*值的选择、距离度量及分类决策规则。

### 1.1 *k*近邻算法

&emsp;&emsp;*k*近邻算法：给定一个训练数据集，对于新的输入实例，在训练数据集中找到与该实例最邻近的*k*个实例，这*k*个实例的多数属于某个类，就把该输入实例分到这个类。

<font size=4 >__算法 1 （*k*近邻法）__</font>
&emsp;&emsp;输入：训练数据集 $T=\left\{\left(x_1,y_1\right),\left(x_2,y_2\right),\cdots,\left(x_N,y_N\right)\right\}$ ，其中，$x_i\in \mathcal{X}=\mathbf{R}^{\text{n}}$ 为实例的特征向量，$y_i\in \mathcal{Y}=\left\{c_1,c_2,\cdots c_K\right\}$ 为实例的类别, $i=1,2,\cdots, N$ ；实例特征向量 $x$；
&emsp;&emsp;输出：实例 $x$ 所属的类别 $y$。

1. 根据给定的距离度量，在训练集 $T$ 中找出与 $x$ 最邻近的 $k$ 个点，涵盖这 $k$ 个点的 $x$ 的邻域记为 $N_k\left(x\right)$；
2. 在 $N_k\left(x\right)$ 中根据分类决策规则决定 $x$ 的类别 $y$
$$
y=\arg \underset{c_j}{\max}\sum_{x_i\in N_k\left( x \right)}{I\left( y_i=c_j \right)},\ i=1,2,\cdots ,N\ ;\ j=1,2,\cdots ,K
$$
式中的 $I$ 为指示函数，当 $y_i=c_j$ 时 $I$ 为1，否则 $I$ 为0.

&emsp;&emsp;*k*近邻法的特殊情况是 $k=1$ 的情况，称为最邻近算法。对于输入的实例点 $x$，最邻近法将训练数据集中与 $x$ 最邻近点的类作为 $x$ 的类。

&emsp;&emsp;需要注意的是，*k*近邻法没有显式的学习过程。

## 二、*k*近邻模型

### 2.1 模型

&emsp;&emsp;*k*近邻算法中，当训练集、距离度量（如欧氏距离）、*k*值以及分类决策规则确定后，对于输入的新的实例，其所属的类唯一确定。相当于根据以上要素将输入的特征空间进行划分，每个实例点（即特征向量）所属的类确定且唯一。

&emsp;&emsp;特征空间中，每个训练实例点 $x_i$，距离该点比其他点更近的点组成了一个区域，叫做单元(*cell*)。每个训练实例点属于一个单元，所有实例点的单元构成特征空间的一个划分。即每个单元中的实例点的类别是确定的，图1-1是二维特征空间划分的一个例子。

<center>
<img src="https://img-blog.csdnimg.cn/20200904135041341.png" width="50%" height="35%">
</center>
<center>
图1-1 *k*近邻法模型对特征空间的一个划分
</center>

### 2.2 距离度量

&emsp;&emsp;特征空间中两个实例点的距离是两个实例点相似程度的反应。*k*近邻模型的特征空间一般是*n*维实数向量空间 $\mathbf{R}^{\text{n}}$ 。使用的距离是欧氏距离，但是也可以使用其他距离，如更一般的 $L_p$距离，或 $\text{Minkowski}$ 距离。
&emsp;&emsp;设特征空间 $\mathcal{X}$ 是 $n$ 维实数向量空间 $\mathbf{R}^{\text{n}}$ ，$x_i,x_j\in \mathcal{X}$，$x_i=\left(x_{i}^{\left( 1 \right)},x_{i}^{\left( 2 \right)},\cdots ,x_{i}^{\left( n \right)}\right)^T$，$x_j=\left(x_{j}^{\left( 1 \right)},x_{j}^{\left( 2 \right)},\cdots ,x_{j}^{\left( n \right)}\right)^T$，$x_i,x_j$的 $L_p$ 距离定义为
$$
L_p\left( x_i,x_j \right) =\left( \sum_{l=1}^n{\left| x_{i}^{\left( l \right)}-x_{j}^{\left( l \right)} \right|^p} \right) ^{\frac{1}{p}}
$$
这里要求$p\ge 1$，当 $p\ge2$ 时，称为欧氏距离(*Eucidean&ensp;distance*)，即
$$
L_2\left( x_i,x_j \right) =\left( \sum_{l=1}^n{\left| x_{i}^{\left( l \right)}-x_{j}^{\left( l \right)} \right|^2} \right) ^{\frac{1}{2}}
$$
当 $p=1$ 时，称为曼哈顿距离(*Manhattan&ensp;distance*)，即
$$
L_1\left( x_i,x_j \right) =\sum_{l=1}^n{\left| x_{i}^{\left( l \right)}-x_{j}^{\left( l \right)} \right|}
$$
当 $p=\infty$ 时，由数学分析的知识可知，是各个坐标距离的最大值，即
$$
L_\infty\left( x_i,x_j \right) =\underset{l}{\max}\left| x_{i}^{\left( l \right)}-x_{j}^{\left( l \right)} \right|
$$
不同距离度量所确定的最邻近点是不同的。

### 2.3 *k*值的选择

&emsp;&emsp;*k*值的选择会对算法产生很大的影响。
&emsp;&emsp;如果选择较小的*k*值，相当于在较小的邻域中选择实例进行训练，学习过程的近似误差会减小，即只与输入实例距离度量较近的训练实例才会起作用；但是学习过程的估计误差会增大，预测结果对近邻的实例点很敏感，如果邻近的实例点恰好是噪声，预测就会出错。换句话说，*k*值选择过小会产生过拟合。
&emsp;&emsp;如果选择较大的*k*值，相当于贼较大的邻域内选择实例进行训练，可以减小学习的估计误差，但是近似误差会增大。*k*值的增大意味着模型变得过于简单，当极端情况 $k=N$ 时，无论输入实例是什么，都将简单地预测属于训练实例中最多的类，即发生的欠拟合的现象。

### 2.4 分类决策规则

&emsp;&emsp;*k*近邻法的分类规则往往是多数表决，即由输入实例的*k*个邻近的训练实例中的多数类决定输入实例的类。
&emsp;&emsp;多数表决规则有如下解释(*majority&ensp;voting&ensp;rule*)：如果分类的损失函数为 $0-1$ 损失函数，分类函数为
$$
f:\mathbf{R}^{\text{n}}\rightarrow \left\{c_1,c_2,\cdots,c_K\right\}
$$
则误分类的概率为
$$
P\left(Y\ne f\left(X\right)\right)=1-P\left(Y=f\left(X\right)\right)
$$
对给定的实例 $x\in \mathcal{X}$，其最邻近的*k*个训练实例点构成集合 $N_k\left(x\right)$。如果涵盖 $N_k\left(x\right)$ 的区域的类别是 $c_j$，那么误分类是
$$
\cfrac{1}{k}\sum_{x_i\in N_k\left( x \right)}{I\left( y_i\ne c_j \right)}=1-\frac{1}{k}\sum_{x_i\in N_k\left( x \right)}{I\left( y_i=c_j \right)}
$$
要使误分类最小化，就要使 $\sum_{x_i\in N_k\left( x \right)}{I\left( y_i=c_j \right)}$ 最大，所以多数表决规则等价于经验风险最小化。

Python代码实现如下：

```python
def calcDist(x1, x2):
    """
    计算两个实例点向量之间的距离，使用的是欧氏距离
    :param x1:向量1
    :param x2:向量2
    :return:向量之间的欧式距离
    """
    return np.sqrt(np.sum(np.square(x1 - x2)))
    # 马哈顿距离计算公式
    # return np.sum(x1 - x2)


def getClosest(trainDataMat, trainLabelMat, x, topK):
    """
    预测实例x的标记。
    获取方式通过找到与实例x最近的topK个点，并查看它们的标签。
    查找里面占某类标签最多的那类标签
    :param trainDataMat:训练集数据集
    :param trainLabelMat:训练集标签集
    :param x:要预测的实例x
    :param topK:选择参考最邻近实例的数目
    :return:预测的标记
    """
    # 建立一个存放向量x与每个训练集中实例距离的列表
    # 列表的长度为训练集的长度，distList[i]表示x与训练集中第i个实例的距离
    distList = [0] * len(trainLabelMat)
    # 遍历训练集中所有的实例点，计算与x的距离
    for i in range(len(trainDataMat)):
        # 获取训练集中当前实例的向量
        x1 = trainDataMat[i]
        # 计算向量x与训练集实例x的距离
        curDist = calcDist(x1, x)
        # 将距离放入对应的列表位置中
        distList[i] = curDist

    # 对距离列表进行排序
    # argsort：函数将数组的值从小到大排序后，并按照其相对应的索引值输出
    topKList = np.argsort(np.array(distList))[:topK]  # 升序排序
    # 建立一个长度为十的列表，用于选择数量最多的标记
    # 采用多数表决的决策规则，topK个标记每人有一票，在数组中每个标记代表的位置中投入
    # 自己对应的地方，随后进行唱票选择最高票的标记
    labelList = [0] * 10
    # 对topK个索引进行遍历
    for index in topKList:
        # trainLabelMat[index]：在训练集标签中寻找topK元素索引对应的标记
        # int(trainLabelMat[index])：将标记转换为int（实际上已经是int了，但是不int的话，报错）
        # labelList[int(trainLabelMat[index])]：找到标记在labelList中对应的位置
        # 最后加1，表示投了一票
        labelList[int(trainLabelMat[index])] += 1
    # max(labelList)：找到选票箱中票数最多的票数值
    # labelList.index(max(labelList))：再根据最大值在列表中找到该值对应的索引，等同于预测的标记
    return labelList.index(max(labelList))


def model_test(trainDataArr, trainLabelArr, testDataArr, testLabelArr, topK):
    """
    测试正确率
    :param trainDataArr:训练集数据集
    :param trainLabelArr: 训练集标记
    :param testDataArr: 测试集数据集
    :param testLabelArr: 测试集标记
    :param topK: 选择多少个邻近点参考
    :return: 正确率
    """
    print('start test')
    # 将所有列表转换为矩阵形式，方便运算
    trainDataMat = np.mat(trainDataArr)
    trainLabelMat = np.mat(trainLabelArr).T
    testDataMat = np.mat(testDataArr)
    testLabelMat = np.mat(testLabelArr).T

    # 错误值技术
    errorCnt = 0
    # 遍历测试集，对每个测试集实例进行测试
    # 由于计算向量与向量之间的时间耗费太大，所以此处只测试200个实例
    for i in range(200):
        # print('test %d:%d'%(i, len(trainDataArr)))
        print('test %d:%d' % (i, 200))
        # 读取测试集当前测试实例的向量
        x = testDataMat[i]
        # 获取预测的标记
        y = getClosest(trainDataMat, trainLabelMat, x, topK)
        # 如果预测标记与实际标记不符，错误值计数加1
        if y != testLabelMat[i]: errorCnt += 1

    # 返回正确率
    return 1 - (errorCnt / 200)

```
