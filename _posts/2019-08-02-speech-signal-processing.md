---
layout: post
title: 【读书笔记】《语音信号处理（C++版）》
tags: [research]
---

## 目录

1. [绪论](#introduction)
2. [基础知识](#basics)
3. [分析方法](#analysis)
4. [特征提取技术](#features)
5. [语音增强](#augmentation)
6. [说话人识别](#speaker)
7. [语音识别](#speech)

<h2 id="introduction"> 1. 绪论 </h2>

#### 语音信号处理的研究方向

- 语音增强
- 说话人识别
- 语音识别
- 语音情感识别
- 语音合成与转换
- 声源定位
- 语音隐藏
- 语音编码
- 声反馈抑制

<h2 id="basics"> 2. 语音信号处理的基础知识 </h2>

#### 语音产生的数学模型

- 激励模型：声门以下负责产生激励振动
  - 浊音激励
  - 清音激励
  - 更严谨一点，应该有浊辅音，浊擦音等
- 声道模型：声门到嘴唇的呼气通道是声道
  - 声管模型
  - 共振峰模型
- 辐射模型：语音从嘴唇辐射出去

#### 语音的常用参数

- 强度与响度
  - 声压与声压级
  - 声强与声强级 `$L_I = 10\lg(I/I_0)$`，其中`$I_0$`为参考声强
  - 响度
  - 等响度曲线
- 频率与音高
  - 客观频率是`$f$`，主观感觉为音高，音高单位为Mel
  - `$\text{Mel}=2595\lg (1+f/700)$`
- 音色与音质

#### 语音信号的表征

- 时域表示
- 频谱表示
- 语谱图

<h2 id="analysis"> 3. 语音信号分析方法 </h2>

#### 语音信号预处理

- 分帧与加窗
  - 矩形窗
  `$$
  w(n) = \left\{\begin{matrix}
  1, & 0\leq n\leq N-1\\ 
  0, & \text{otherwise}.
  \end{matrix}\right.
  $$`
  - 汉宁窗
  `$$
  w(n) = \left\{\begin{matrix}
  0.5(1-\cos(2\pi n/(N-1))), & 0\leq n\leq N-1\\ 
  0, & \text{otherwise}.
  \end{matrix}\right.
  $$`
  - 汉明窗
  `$$
  w(n) = \left\{\begin{matrix}
  0.54-0.46\cos(2\pi n/(N-1)), & 0\leq n\leq N-1\\ 
  0, & \text{otherwise}.
  \end{matrix}\right.
  $$`
- 消除趋势项和直流分量
- 预加重与去加重pre-emphasis

#### 语音信号的时域分析

#### 语音信号的频域分析

#### 语音信号的倒谱分析

#### 语音信号的线性预测分析

<h2 id="features"> 4. 语音信号特征提取技术 </h2>

本章主要介绍三种主要的语音信号特征：语音端点、基音周期和共振峰。

#### 端点检测（voice activity detection）

- 双门限法
  - 短时能量检测为主，短时过零率检测为辅
- 自相关法
- 谱熵法
  - **谱熵定义**

设语音信号时域波形为`$x(i)$`，加窗分帧后第`$n$`帧语音信号为`$x_n(m)$`，其FFT为`$X_n(k)$`。则对于某一谱线`$k$`的能量谱为`$Y_n(k)=X_n(k)X_n^*(k)$`。该语音帧的**短时能量**为
`$$
E_n=\sum_{k=0}^{N/2}Y_n(k)
$$`
其中，N为FFT的长度，只取正频率部分。每个频率分量的**归一化谱概率密度函数**为
`$$
p_n(k)=\frac{Y_n(k)}{E_n}
$$`
该语音帧的短时**谱熵**定义为
`$$
H_n=-\sum_{n=0}^{N/2}{p_n(k)\lg p_n(k)}
$$`
- 比例法
- 谱距离法

#### 基音周期估计

- 概述
  - 基音周期估计作为语音信号处理中描述激励源的重要参数之一，在语音合成、语音压缩编码、语音识别和说话人确认等领域都有着广泛而重要的用途，***尤其对汉语语音的理解更是如此***。
  - 汉语是一种有调语言，而基音周期的变化称为声调，声调对于汉语语音的理解非常重要。
  - 目前的基音检测算法大致可分为两大类：1. 基于事件检测方法（计算量大） 2. 非基于事件检测方法（算法简单，运算量小）。这里的事件是指**声门闭合**。
- 信号预处理
  - 基音周期估计需要前置VAD，而且对VAD要求更严格，常采用基于谱熵比法的VAD。此处的VAD只能用一个门限`$T_1$`，还要进一步判断语音段的长度是否大于minL，这里minL一般设定为10帧。
  - 预滤波器：选择带宽一般为60~500 Hz。（如果考虑共振峰的影响，上限频率可以增大到900 Hz）
  - 考虑到语音信号对相位不敏感，因此选择运算量小的椭圆IIR滤波器。
- 自相关法（属于非基于事件的检测方法）
  - 窗函数应该选矩形窗，窗长要合适
  - 减小共振峰的影响：1. 使用60~900 Hz的带通滤波 2. 使用非线性变换：**中心削波**, 为减小计算量，可以考虑**三电平中心削波**
- 平均幅度差函数法（属于非基于事件的检测方法）
- 倒谱法（属于非基于事件的检测方法）
  - 倒谱法是传统的基音周期检测算法之一
  - 语音加窗的选择很重要，一般选汉明窗
- 简化逆滤波法
  - 自相关处理法的一种现代化版本
- 基音检测后处理
  - 中值平滑处理
  - 线性平滑处理
  - 组合平滑处理

#### 共振峰估计

<h2 id="augmentation"> 5. 语音增强 </h2>

#### 基础知识
- 人耳感知特性
  - 频谱敏感、相位不敏感，
  - 对100Hz以下的低频声音不敏感、对高频声音尤其是2000~5000Hz的声音敏感(3000Hz最敏感)
  - 人耳对频率的分辨能力受声强的影响
  - 具有掩蔽效应：低声强的频率成分会被高声强的频率成分掩蔽
  - 具有选择性注意特性：嘈杂环境下能将注意力集中在感兴趣的声音上
- 语音特性
- 噪声特性
  - 加性噪声：带噪语音是语音和噪声的和
  - 非加性噪声：带噪语音不是语音和噪声的和。如：传输噪声在时间域里是语音和噪声的卷积。非加性噪声可以通过同态处理转换为加性噪声。
- 语音质量评价标准
  - 主观评价：清晰度或可懂度和音质两类
  - 客观评价：基于信噪比的评价方法，基于谱距离的评价方法，基于听觉模型的评价方法

#### 谱减法
- 谱减法是处理宽带噪声较为传统和有效的方法，其基本思想是在假定加性噪声与短时平稳的语音信号相互独立的条件下，从带噪语音的功率谱中减去噪声功率谱。
- 改进算法：

#### 维纳滤波法

#### 自适应滤波法

- 最小均方误差滤波器 (LMS)
  - 算法实现简单，不依赖模型，性能稳健，因此实际应用比较成功
- 归一化最小均方误差滤波器 (NLMS)
  - 对于LMS来说，正值的步长将影响权矢量收敛到误差曲面极小点的速率。使用归一化步长避免这个问题。
- 自适应陷波器
- 干扰抑制

#### 基于听觉掩蔽效应的语音增强方法

<h2 id="speaker"> 6. 说话人识别 </h2>

#### 识别原理和系统结构
- 端点检测的成功率对语音识别系统的成败有很大影响
- 特征选取：倒谱，差值倒谱，基音，差值基音
- 特征参量评价方法：F比=不同说话人特征参数均值的方差均值 / 同一说话人特征的方差均值
- 模式匹配方法：DTW，矢量量化方法(VQ)，HMM，GMM，人工神经网络方法(ANN)

#### 应用VQ的说话人识别系统
- 当训练数据量较小时，基于VQ的方法比连续的HMM方法有更大的鲁棒性
  - 失真测度：欧氏距离测度，线性预测失真测度，识别失真测度
  - 训练：LBG算法

#### 应用GMM的说话人识别系统
在辨认任务中，目的是找到一个说话者`$i$`，其对应的模型参数`$\theta_i$`使得待识别语音特征矢量组`$\boldsymbol{X}$`具有最大后验概率`$P(\theta_i/\boldsymbol{X})$`。
根据贝叶斯理论，最大后验概率可表示为
`$$
P(\theta_i/\boldsymbol{X})=\frac{P(\boldsymbol{X}/\theta_i)P(\theta_i)}{P(\boldsymbol{X})}
$$`
假定该语音信号出自封闭集里的每个人的可能性相等，也就是说
`$$
P(\theta_i)=1/N, i\in[1,N]
$$`
对于一个确定的观察值矢量`$\boldsymbol{X}$`，`$P(\boldsymbol{X})$`是一个确定的常数值。这样问题转化为
`$$
i = \arg \max_i P(\boldsymbol{X}/\theta_i)
$$`

- 多维高斯（正态）分布概率密度函数，观察值矢量维度为`$d$`
`$$
N(\boldsymbol{x};\boldsymbol{\mu},\boldsymbol{\Sigma})=
\frac{1}{\sqrt{(2\pi)^d|\boldsymbol{\Sigma}|}}
\exp\left\{-\frac{1}{2}
(\boldsymbol{X}-\boldsymbol{\mu})^T
\boldsymbol{\Sigma}^{-1}
(\boldsymbol{X}-\boldsymbol{\mu})
\right\}
$$`
- 高斯混合分布 (GMM)
`$$
p(x^{(i)})=\sum_{j=1}^M\alpha_j N_j(x^{(i)};\boldsymbol{\mu}_j,\boldsymbol{\Sigma}_j)
$$`
其中影响因子`$\alpha_j$`满足
`$$
\sum_{j=1}^M \alpha_j = 1
$$`
GMM共有M个单高斯模型。
- GMM初始化：k均值聚类
- GMM收敛条件的选择

<h2 id="speech"> 7. 语音识别 </h2>

#### 识别原理与系统构成
- 由于汉语的音位变体过于复杂，因此不宜选用音素作为识别单元。采用声母和韵母作为识别的参数基元、以音节字为识别单元，结合同音字理解技术以及词以上的句子理解技术的一整套策略，可望实现汉语全字（词）语音识别和理解的目的。
- 选择k-means门限值是问题的关键？(p179)

#### HMM算法
- 算法的改进：一般来说，初始概率和状态转移系数矩阵的初值较易确定。而发射概率B的初值设置较其他两组参数的设置更重要，也更困难。可以使用k-means算法估计B。

#### 性能评测
