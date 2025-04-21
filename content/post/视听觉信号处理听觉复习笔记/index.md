---
title: 视听觉信号处理听觉复习笔记
draft: false
math: true
date: 2020-12-19 15:53:28
cover: images/featureimages/11.jpg
summary: 2020秋季学期视听觉信号处理听觉复习笔记
tags: [学长的火炬,视听觉信号处理]
categories: [视听觉信号处理]
---

## 前言

教务处的垃圾排课，非要单双周上课，结果听觉上完课的两天后就考试，就nm离谱，被迫现在开始复习。

## 总述

课程主要内容：

- 语音识别(Speech Recognition)
- 语音编码(Speech Coding)

## 一、语音的声学表示

### 1.1 语音的声学特性

&emsp;&emsp;语音以声波的形式在空气中传播，声波是纵波，振动方向与传播方向一致。

### 1.2 声波的基本物理量

1. 频率：人耳对声波频率高低的感觉与实际频率近似成对数关系，即“分贝”的由来。**基频：60Hz~500Hz**。

2. 振幅:

- 用声压或声强表示声音的强度
- 声压p用来度量由于声波的传播导致的气压的变化，单位帕斯卡
- 声强 I 为单位时间内通过与声波传播方向垂直的某一单位面积上的声能的平均值，单位$\left.W /m^2 \right.$.
- 最小声压：闻阈，约为$2\times 10^{-5}Pa$；
- 最大声压：痛阈，约为$200Pa$.

3. 分贝：

- 以闻阈$P_0$为基准，单位dB，
- 依照声压定义分贝：$L=20\log_{10}\left(P\left. / P_0\right.\right)\left(\mathrm{dB}\right)$
- 依照声强定义分贝：$L=10\log_{10}\left(I\left. / I_0\right.\right)\left(\mathrm{dB}\right)$
- 闻阈相当于 $0\mathrm{dB}$，痛阈相当于 $140\mathrm{dB}$，人讲话时，一米远处的声强大约为$60dB \sim80dB$。

4. 共振和共振峰

- 共振：物体受迫振动时，所加驱动频率等于物体固有频率时，物体以最大振幅振动。
- 元音的音色和区别特征主要取决于声道的共振峰特性。

## 二、语音的数字信号表示

### 2.1 数字信号处理

就nm躲不过了呗。

1. 离散时间信号与系统

- 在时间上离散，只在某些不连续的规定瞬间给出函数值。幅值连续—抽样信号，幅值离散—数字信号。
- 通常函数值离散时刻之间的间隔是均匀的，以序列 $x\left(n\right)\; \left(n=0, \pm1,\pm2,\cdots\right)$表示。

2. 离散信号序列的基本运算

序列中同序号的数值逐项运算构成一个新序列。
<div style="text-align:center">

| 运算       | 规则         |
|:-----------:|:-------------:|
| 加 | $z\left(n\right) = x\left(n\right)+y\left(n\right)$  |
| 乘 | $z\left(n\right) = x\left(n\right)\cdot y\left(n\right)$ |
| 时延 | $z\left(n\right) = x\left(n-m\right)$ |
| 求能量 | $E = \sum\limits_{n=-\infty}^{\infty}\lvert x\left(n\right) \rvert^{2}$ |

</div>

3. 离散时间系统：系统的激励和相应都是离散信号序列。离散时间系统的数学模型是差分方程。

- 常见为线性时不变系统，设$y_1\left(n\right)$和$y_2\left(n\right)$分别为激励$x_1\left(n\right)$和$x_2\left(n\right)$的响应，则线性系统有激励$\alpha x_1\left(n\right)+\beta x_2\left(n\right)$，响应为$\alpha y_1\left(n\right)+\beta y_2\left(n\right)$；时不变系统有激励$x\left(n-k\right)$，响应为$y\left(n-k\right)$。

4. 卷积、滤波、离散傅里叶变换（Discrete Fourier Transform）略过。

5. Z变换：

- 将离散系统的数学模型（差分方程）转化为代数方程。
- Z变换定义：$X\left(z\right) = \sum\limits_{n=-\infty}^{\infty}x\left(n\right)z^{-n}$
- Z变换在其收敛域$R_1<\lvert z \rvert<R_2$内满足：$\sum_{n=-\infty}^{\infty}\lvert x\left(n\right) \rvert \lvert z \rvert^{-n} < \infty$
- **DFT为特殊的Z变换**，取$z = \exp \left(j\left(\left.2\pi/N\right.\right)k\right)$即可得到DFT变换。在z平面的单位圆上，取辐角为$\omega = \left. 2\pi k/N \right.$计算其Z变换，得到了DFT的第k个样值点。**有限长序列的DFT可以解释为其Z变换在单位圆上的均匀抽样**。
- Z变换满足线性可加性，位移性$Z\left[x\left(n-m\right)\right] = z^{-m}X\left(z\right)$和时域的卷积定理$Z\left[x\left(n\right)\ast y\left(n\right)\right] = X\left(z\right)Y\left(z\right)$。
  
6. 离散余弦变换（**D**iscrete **C**osine **T**ransform）：

- $C\left(k\right) = \sum\limits_{n=0}^{N-1}x\left(n\right)\cos\left(\pi k\left.\left(\left. n+1 /\right. 2\right) /\right. N\right)\quad \left(0\le k\le N-1\right)$
- $x\left(n\right) =  \left.\left[C\left(0\right)+2\sum\limits_{k=1}^{N-1}C\left(k\right)\cos\left(\pi k\left. \left(\left. n+1 / \right.2 \right)/ \right. N \right)\right] /\right.N \quad \left(0\le n\le N-1\right)$
- DCT优点是能量集中，相对于DFT，系数主要集中在维数较低的部分，可以用更少的系数逼近原来的信号。

## 三、语音的语言表示

### 3.1 语音的语言表示

1. 句子$\Rightarrow$短语$\Rightarrow$词语$\Rightarrow$音节$\Rightarrow$音素。
2. 音素是语音的基本单位，分为元音（浊音）和辅音（清音）。
3. 还有半元音、双元音、半辅音等。

### 3.2 不同语言的语音表示

1. 美国英语42个音素：元音12个；双元音6个；半元音4个；辅音20个。
2. 汉语音素为64个，分为辅音、单元音、复元音和复鼻元音。

## 四、语音的时频域分析

### 4.1 特征的分析时长

#### 4.1.1 短时分析

&emsp;&emsp;语音信号是非平稳信号，但是在10ms~30ms内，认为语音信号为平稳信号。

&emsp;&emsp;短时分析的最基本手段是对语音信息加窗。
$$S_w\left(n\right) = s\left(m\right)w\left(n\right)$$

常见的窗函数：N为窗长。
- 方窗：$w\left(n\right) = \left\{ \begin{array}{l}
	1\quad 0\le n\le N-1\\
	0\quad 其它\\
\end{array} \right. $

- 哈明窗
- 哈宁窗

### 4.2 特征的时域分析方法

1. 短时能量：

&emsp;&emsp;短时能量可以用于**清浊判断**、**有声段和无声段判定**、**声母韵母分界**以及**连字的分界**，经常是识别系统中特征的一维。

计算公式：
$$E_n = \sum\limits_{m=-\infty}^{\infty}\left[x\left(m\right)w\left(m\right)\right]^2$$

2. 短时平均幅度：

&emsp;&emsp;单位时间内过零发生的次数。

公式：
$$Z_n = \sum_{m=-\infty}^{\infty}\lvert \text{sgn}\left[x\left(m\right)\right] - \text{sgn}\left[x\left(m-1\right)\right] \rvert w\left(m\right)$$

式中：
$$\text{sgn}\left[x\left(n\right)\right] = \left\{ \begin{array}{l}
	1\quad x\left(n\right)\ge0\\
	-1\quad x\left(n\right)<0\\
\end{array} \right.,\quad
w\left(n\right) = \left\{ \begin{array}{l}
	\left.1/ \right.2N\quad 0\le n\le N-1\\
	0\quad 其它\\
\end{array} \right.
$$

&emsp;&emsp;短时平均过零率容易受到低频的干扰，因而提出了门限过零率的思想。

计算公式：
$$Z_n = \sum_{m=-\infty}^{\infty}\left\{\lvert \text{sgn}\left[x\left(m\right)-T\right] - \text{sgn}\left[x\left(m-1\right)-T\right] \rvert +
\lvert \text{sgn}\left[x\left(m\right)+T\right] - \text{sgn}\left[x\left(m-1\right)+T\right] \rvert\right\} w\left(m\right)
$$


#### 4.2.1 端点检测

1. 端点检测是对语音进行“浊音/清音/无音的判定”。
2. 浊音的能量高于清音，清音的过零率高于无声段。
3. 双门限法：
- 用较高的短时门限能量门限$M_1$确保$A_1\thicksim A_2$肯定是浊音；
- 从$A_1$和$A_2$开始向两端搜索，短时能量>较低门限$M_1$的$B_1\thicksim B_2$还是语音段；
- 从$B_1$开始向前搜索，短时过零率<门限$Z_s$的为清音段。

#### 4.2.2 短时自相关函数

1. 自相关函数

- 对于确定的离散信号$x\left(n\right)$，自相关函数定义为
$$R\left(k\right) = \sum_{m=-\infty}^{+\infty}x\left(m\right)x\left(m+k\right)$$
- $R\left(k\right)$表示一个信号和延迟*k*点后

## 五、语音编码

### 5.1 语音编码总述

1. 语音数据大小:
- fs：每秒钟样本数（8k~44.1k）；
- c：每个样本的通道数（1或2）；
- b：每个样本点的位数（8或16）。

2. 比特率：
$$fs\times c \times b$$
单位 $\text{bit/s}$，或bps。

3. 为什么语音可以压缩：

- 语音存在冗余度
  - 幅度非均匀分布
  - 语音信号样本间的相关性很强
  - 浊音具有准周期
  - 声道的形状及其变化缓慢
  - 语音间隙
- 人的听觉感知机理
  - 人的听觉特性具有掩蔽效应
  - 人耳对不同频段声音的敏感程度不同
  - 人耳对语音相位不敏感

4. 编码类型：

- 波形编码：根据语音信号的波形导出相应的数字编码的形式，目的是尽量保持波形不变，使接收端能再现原始语音。抗噪性强，语音质量好；但是需要较高的码率，一般为$16\sim64Kbps$。
- 参数编码：通过对语音信号进行分析，提取参数，对参数进行编码。接收端能用解码后的参数重构语音信号，参数编码主要从听觉感知的角度重现语音，并不保证波形相同，参数编码对码率的要求比波形编码低得多。
- 混合编码：上述两种方法的有机组合，既增加了语音的自然度，提高语音质量；也相对于波形编码实现较低的数码率指标。

### 5.2 波形编码

#### 5.2.1 均匀量化PCM

&emsp;&emsp;最直接的方法，采样量化的A/D转换。采样时，采样频率要高于信号最高频率的两倍，一般采样前还需要低通滤波控制信号的最高频率。量化时将采样得到的样本的幅度用均匀量化的方法表示成二进制数字信号，相当于用一组二进制脉冲序列表示各量化后采样值。这就是脉冲编码调制方法(PCM)。

&emsp;&emsp;量化过程不可避免会产生误差，量化误差$e\left(n\right)$定义为：
$$e\left(n\right) = \bar{x}\left(n\right) - x\left(n\right)$$

#### 5.2.2 非均匀量化PCM

&emsp;&emsp;均匀量化PCM的主要问题是编码速率过高。为了防止幅度较大的信号因为超出量化范围而出现过载，必须使用较高的量化比特数。因此需要进行非均匀量化。对小幅度样本使用小的量化间隔，可以精确量化；对大幅度样本使用大的量化间隔，既可以成功地提高信噪比，又可避免大新号的过载。

&emsp;&emsp;最常用的非均匀量化为对数压扩方法。编码时，利用语音信号的幅度统计特性，对幅度按对数变换进行压缩，然后再进行均匀量化。解码时，进行逆向的扩张变换。有不同的变换方法，如$\mu$律变换、A律变换等。

#### 5.2.3 自适应化PCM

&emsp;&emsp;核心思想是对短时能量较大的信号，采用较大量化间隔进行量化；对短时能量较小的信号，采用较小的量化间隔进行量化，有助于减小量化噪声，提高量化后信号的信噪比。

&emsp;&emsp;除了采用量化间隔作为量化器的特性，还可以采用放大器增益来作为量化器特性，对能量较大的信号采用较小的放大增益，对能量较小的信号采用较大的放大增益。由以上可知，APCM除了发送量化结果之外，还需要发送自适应的调整参数作为边信息。

#### 5.2.3 差分脉冲编码

&emsp;&emsp;由于相邻采样值之间的差值远小于采样值本身，可以对差值进行编码，而不是对采样值本身进行编码，这就是差分脉冲编码DPCM。

&emsp;&emsp;产生差分信号：直接存储前一次的采样值，用本次的采样值计算差值，经过量化得到数字语音编码。解码端做相反处理，恢复原信号。

&emsp;&emsp;用Z变换考查各点信号的时域关系，有
$$C\left(z\right) = X\left(z\right)\left(1-z^{-1}\right) +E\left(z\right)$$
和
$$\overline{X}\left(z\right) = \frac{C\left(z\right)}{1-z^{-1}} = X\left(z\right) + \frac{E\left(z\right)}{1-z^{-1}}$$
式中，$E\left(z\right)$为量化器量化噪声$e\left(n\right)$的Z变换。

&emsp;&emsp;由于量化器的量化噪声被累积叠加到了输出信号中，即每次的量化噪声信号都被保存，叠加到下次的输出当中，如果量化噪声始终是同一方向，则输出信号会越来越偏离原信号。因此，**编码器应采用前一次解码后的采样值代替前一次的输入采样值**，以生成差分信号。

则
$$\widetilde{X}\left(z\right) = \frac{C\left(z\right)z^{-1}}{1-z^{-1}}$$
编码结果为
$$C\left(z\right) = X\left(z\right) - \widetilde{X}\left(z\right)+E\left(z\right)$$
代入后有
$$C\left(z\right) = \left(X\left(z\right)+E\left(z\right)\right)\left(1-z^{-1}\right)$$

因此有
$$\overline{X}\left(z\right) = \frac{C\left(z\right)}{1-z^{-1}} = X\left(z\right) + E\left(z\right)$$

由此可见，已经消除了量化噪声的累积。

&emsp;&emsp;DPCM仅仅用到了两个采样值之间的相关性。然而，当前输入采样不仅与上一时刻的采样值相关，也与前面若干采样值相关，可以使用线性预测分析的方法实现一般形式的差分脉冲编码。根据线性预测分析的原理，可以用过去的一些采样值的线性组合来预测和推断当前的采样值，得到一组线性预测系数，且预测误差$e\left(n\right)$的动态范围和平均能量均比信号$x\left(n\right)$小得多，预测阶数越高，预测误差越小，相应的编码速率可以越低。

#### 5.2.4 自适应差分脉冲编码ADPCM

&emsp;&emsp;由于语音信号的不稳定，不能保证其总是最佳线性预测器，从而使得线性误差最小。一种较好的方法是，在编码过程中，采用自适应技术动态调整预测器系数。或者采用自适应量化技术对差分信号进行量化，也可以降低编码速率。

&emsp;&emsp;前馈型ADPCM的原理如图所示：

![APPCM原理](./2020秋机器学习期末试题/adpcm.png)







