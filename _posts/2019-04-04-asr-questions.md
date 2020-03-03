---
layout: post
title: 语音识别的基本问题与基本概念解析
tags: [asr]
---

## 1. 语音识别的三个基本问题

语音识别：将语音转写为文字，涉及到三个基本问题。

1. 前后上下文具有相关性
2. 语音和文本的长度关系可变（参考[《语音识别基本法》](http://cslt.riit.tsinghua.edu.cn/mediawiki/index.php/Speech_book) Version 20190126 第2.2.1小节）
3. 基本建模单元的粒度大小（参考[《语音识别基本法》](http://cslt.riit.tsinghua.edu.cn/mediawiki/index.php/Speech_book) Version 20190126 第2.2.1小节）

## 2. 基本概念

我们通常所说的语音识别一般指大语汇量连续语音识别，即large vocabulary continuous speech recognition (LVCSR)。

### 语音

- mel-cepstral distortion
- mel-scale log filter banks
- *梅尔倒谱系数mel-frequency cepstral coefficients (MFCC): a good descriptor for ASR
- *感知线性预测特征PLP
- 线性判别分析技术(Linear Discriminant Analysis, LDA)
Ref. 张俊博的博士论文第三章
- MOS score: mean opinion score
- infinite impulse response (IIR) filter vs. finite impulse response (FIR) filter
- MMI based sequence training
- Back-propagation through time (BPTT)
- n-gram language model
- Lombard Effect of noisy speeches
- Accents database: <http://www.voxforge.org>

### 隐马尔可夫模型

- 隐马尔可夫模型是一个带概率的有限状态隐马尔可夫链，也可认为是一个离散时域有限状态自动机(FSA)。（Ref. 张俊博的博士论文第21页）
- 隐马尔可夫模型的评价、解码和训练算法分别是前向算法，维特比算法和Baum-Welch算法。(Ref. 张俊博thesis）
- 维特比算法是一种动态规划算法。（Ref. 张俊博thesis)
- 一文搞懂隐马尔可夫模型: <https://www.cnblogs.com/skyme/p/4651331.html>

### 解码

- 维特比算法
- 对于小词汇量的语音识别系统，在解码时只要将字典的词条展开词环就可以进行解码;
- LVCSR：基于树形结构的解码方法以及带权有限状态机的解码方法。(Ref. 徐海华thesis)
- 大词汇量连续语音识别中，ROVER是最早被提出的多系统联合解码方法
- 对齐的问题？

### 盲源分离算法

独立成分分析(Independent Component Analysis)是解决盲信号分离的典型方法。(Ref. 崔浩的硕士论文第三章)

### 文献阅读

- Supervised Sequence Labelling with RNN (Graves)
  - Ch. 2: Supervised Learning, Supervised Pattern Classification, Supervised Sequence Labelling
    - Pattern Classification (Recognition): Multilayer Perceptrons, Support Vector Machines.

## 3. 现有疑惑

1. Lexicon也许不是那么简单的事情？(Ref. Prabhavalkar, Interspeech 2018 presentation)
2. LDA用来降维？(A keyword search system using open source software)
