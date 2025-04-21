---
title: 模型评估与选择
draft: false
math: true
date: 2020-08-03 19:44:23
cover: images/featureimages/2.jpg
summary: 关于机器学习中模型评估以及选择的总结
tags: [算法,机器学习]
categories: [机器学习]
---


## 一、经验误差与过拟合

### 1.1 一些概念

1. 错误率：分类错误的样本数占样本总数的比例。如果在*m*个样本中有*a*个样本分类错误，则错误率为$E=\left. a \middle/ m \right.$ ；相应的，$1-\left. a\middle/ m \right.$ 称为“精度”。

2. 误差：学习器的实际预测输出与样本的真实输出之间的差异；学习器在训练集上的误差称为“训练误差”或“经验误差”；在新样本上的误差称为“泛化误差”。

### 1.2 过拟合与欠拟合

#### 1.2.1 过拟合(overfitting)与欠拟合(underfitting)

&emsp;&emsp;不用晦涩难懂的定义了，通俗解释一下。为了希望得到在新样本上表现很好的学习器，应该从训练样本中尽可能的学习适用于所有潜在样本的“普遍规律”。当把训练样本学得“太好了”的时候，很可能把训练样本自身的一些特点当做了所有潜在样本都会具有的性质，这样会导致泛化性能下降，这就是“过拟合”。
&emsp;&emsp;欠拟合正好相反，即训练样本的一般性质尚未学好。

#### 1.2.2 解决办法

1. 欠拟合解决办法
   - 在决策树学习中扩展分支
   - 在神经网络中增加训练轮数

2. 过拟合问题
&emsp;其实过拟合无法完全消除，只可进行“缓解”。因为机器学习面临的问题通常是*NP*难的问题，有效的学习算法必然是在多项式时间内运行完成。如果可以彻底避免过拟合，则通过经验误差最小化即可得到最优解，这就意味着构造性的证明了$P = NP$。因此，只要相信$P \ne NP$ ，过拟合就无法避免。

## 二、评估方法

&emsp;&emsp;需要选择“测试集”来测试学习器对新样本的判别能力，以测试集上的“测试误差”作为泛化误差的近似。通常假设测试样本是从样本的真实分布中独立同分布采样而得。但是，测试集应该尽可能与训练集互斥，让测试样本尽量不在训练集中出现、未在训练过程中使用过。
&emsp;&emsp;为了对数据集*D*做适当处理，得到训练集*S*和测试集*T*。下面介绍几种常见方法。

### 2.1 留出法

&emsp;&emsp;“留出法”(*hold-out*)直接将数据集*D*划分为两个互斥的集合，其中一个作为训练集*S*，一个作为测试集*T*，即$D=S \cup T$ , $S \cap T=\varnothing$，在S上训练模型，用T评估测试误差，作为对泛化误差的估计。

注意要点：

1. 训练集/测试集的划分要尽可能保持数据分布的一致性，避免因为数据划分而引入的额外偏差对最终结果产生影响。例如可以采取“分层抽样”等一系列保留类别比例的抽样方法。
2. 不同的划分会导致不同的训练/测试集，相应的模型评估也有差别。因此，单次使用的留出法得到的结果往往不稳定可靠。所以在使用留出法时，一般采取若干次随机划分、重复实验评估取平均值作为留出法的评估结果。
3. 如果训练集*S*包含绝大多数样本，则训练出的模型可能更接近于用*D*训练出的模型，但是由于*T*较小，评估结果可能不够稳定准确；若令测试集T包含过多的样本，则*S*与*D*的差别更大了，被评价的模型与用*D*训练出的模型可能有较大差别。因此常见做法是：测试集至少包含30个测试用例，且数据集的大约$\left. 2 / 3 \right. \sim \left. 4 / 5 \right.$的样本用于训练，剩余样本用于测试。

### 2.2 交叉验证法

&emsp;&emsp;“交叉验证法”(*cross&ensp;validation*)先将数据集*D*划分成*k*个大小相似的互斥子集，即$D=D_1\cup D_2\cup \cdots \cup D_k$，$D_i\cap D_j=\varnothing \left(i\ne j\right)$。每个子集$D_i$都尽可能保持数据分布的一致性。
&emsp;&emsp;每次使用$k-1$个子集的并集作为训练集，余下的子集作为测试集；这样可以得到*k*组训练/测试集，进行*k*次训练和测试，最终返回*k*次测试结果的均值。

### 2.3 自助法

&emsp;&emsp;我们希望评估的是用*D*训练出的模型，但是在以上两种方法中，由于必然会保留一部分样本用于测试，因此实际的训练模型所使用的训练集比*D*小。因此必然会引入因为训练样本规模不同而导致的估计偏差。

&emsp;&emsp;“自助法”(*bootstrapping*)以自助采样阀为基础。给定包含*m*个样本的数据集*D*，采样产生数据集*D'*：每次随机从*D*中挑选一个样本，拷贝放入*D'*，再将样本放回初始数据集*D*中，使该样本在下次采样时仍有可能被取到；重复执行*m*次后，得到了包含*m*个样本的数据集*D'*。显然，*D*中的一部分样本会在*D'*中多次出现，而另一部分样本不出现。

&emsp;&emsp;自助法在数据集小、难以有效划分训练、测试时有用；此外，自助法能从初始数据集中产生多个不同的训练集，对继承学习等方法有很大的好处。

## 三、性能度量

&emsp;&emsp;对学习器的泛化性能进行评估，不仅需要有效可行的实验估计方法，还需要有衡量模型泛化能力的评价标准，即性能度量(*performance&ensp;measure*)。
&emsp;&emsp;下面介绍常用的性能度量。

### 3.1 均方误差

&emsp;&emsp;回归任务最常用的性能度量是“均方误差”$$E\left( f;D \right) =\frac{1}{m}\sum_{i=1}^m{\left( f\left( \boldsymbol{x}_i \right) -y_i \right) ^2}$$

&emsp;&emsp;更一般的，对于数据$\mathcal{D}$和概率密度函数$p\left(\cdot\right)$，均方差可描述为$$E\left(f;D\right)=\int_{\boldsymbol{x}\sim\mathcal{D}}{\left(f\left(\boldsymbol{x}\right)-y\right)^2p\left(\boldsymbol{x}\right)}\text{d}x$$

### 3.2 错误率与精度

&emsp;&emsp;错误率与精度适用范围较广，既适用于二分类任务，也适用于多分类任务。

1. 错误率：分类错误的样本数占样本总数的比例。
   - 对于样例集*D*，分类错误的定义为：$$E\left( f;D \right) =\frac{1}{m}\sum_{i=1}^m{\mathbb{I}\left( f\left( \boldsymbol{x}_i \right) \ne y_i \right)}$$
   - 对于数据分布$\mathcal{D}$，和概率密度$p\left(\cdot\right)$，错误率定义为$$E\left( f;D \right) =\int_{\boldsymbol{x}\sim \mathcal{D}}{\mathbb{I}\left( f\left( \boldsymbol{x} \right) \ne y \right) p\left( \boldsymbol{x} \right)}\text{d}x$$
2. 精度：分类正确的样本数占样本总数的比例。
   - 对于样例集*D*，精度的定义为：
   $$
   \begin{aligned}
       \text{acc}\left( f\ ;D \right) &= \frac{1}{m}\sum_{i=1}^m{\mathbb{I}\left( f\left( \boldsymbol{x}_i \right) =y_i \right)} \\
       &= 1-E\left( f\ ;D \right)
   \end{aligned}
   $$
   - 对于数据分布$\mathcal{D}$，和概率密度$p\left(\cdot\right)$，精度为：
   $$
   \begin{aligned}
      \text{acc}\left( f\ ;D \right) &= \frac{1}{m}\sum_{i=1}^m{\mathbb{I}\left( f\left( \boldsymbol{x}_i \right) =y_i \right)} \\
       &= 1-E\left( f\ ;D \right)
   \end{aligned}
   $$

### 3.3 查准率、查全率与$F_1$

&emsp;&emsp;在信息检索中，我们会关心“检索出的信息中有多少比例是用户感兴趣的”“用户感兴趣的检索出来了多少”。

&emsp;&emsp;对于二分类问题，将样例根据其真实类别与学习器预测类别的组合划分为真正例(*true&ensp;positive*)、假正例(*false&ensp;positive*)、真反例(*true&ensp;negative*)、假反例(*false&ensp;negative*)。分类结果如表所示
<center>
   <table class="tg" align="center">
      <thead>
         <tr>
            <th class="tg-c3ow" rowspan="2">真实情况</th>
            <th class="tg-c3ow" colspan="2">预测结果</th>
         </tr>
         <tr>
            <td class="tg-0pky">正例</td>
            <td class="tg-0pky">反例</td>
         </tr>
      </thead>
      <tbody>
         <tr>
            <td class="tg-c3ow">正例</td>
            <td class="tg-0pky">TP(真正例)</td>
            <td class="tg-0pky">FN(假反例)</td>
         </tr>
         <tr>
            <td class="tg-c3ow">反例</td>
            <td class="tg-0pky">FP(假正例)</td>
            <td class="tg-0pky">TN(真反例)</td>
         </tr>
      </tbody>
   </table>
</center>

&emsp;&emsp;查准率*P*与查全率*R*分别定义为$$P=\cfrac{TP}{TP+FP}$$ $$R=\cfrac{TP}{TP+FN}$$

&emsp;&emsp;一般来说，查准率高，查全率往往偏低；查全率高，查准率往往偏低。通常只在一些简单的任务中，才能使得查全率和查准率都很高。平衡点(*Break-Even&ensp;Point*，简称*BEP*)，是“查准率=查全率”时的取值。

&emsp;&emsp;$F_1$度量：$$F_1=\cfrac{2\times P\times R}{P+R}=\cfrac{2\times TP}{样例总数+TP-TN}$$

&emsp;&emsp;$F_1$度量的一般形式为$F_\beta$，能表达出对查准率/查全率的不同偏好，定义为$$F_\beta=\cfrac{\left(1+\beta^2\right)\times P\times R}{\left(\beta^2\times P\right)+R}$$，其中$\beta>0$度量了查全率对查准率的相关重要性，$\beta=1$退化为标准的$F_1$；$\beta>1$时查全率有更大的影响；$\beta<1$时查准率有更大的影响。

