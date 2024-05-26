---
layout: post
title: CCF语音对话与听觉前沿研讨会
tags: [research]
---

Video: <https://www.bilibili.com/video/BV1MV411k7iJ>

主持人：贾珈（清华大学, [个人主页](https://hcsi.cs.tsinghua.edu.cn)），谢磊（西北工业大学, [个人主页](http://lxie.npu-aslp.org)）& 欧智坚（清华大学, [个人主页](http://oa.ee.tsinghua.edu.cn/~ouzhijian)）

## 1. Recent Advances in Speaker Extraction

- Speaker: 李海洲（NUS）
- Time Stamp: 25:00

### Selective auditory attention

#### Ears and Hearing

- Frequency Analyser
- Localization/Equilibrium
- Attention (跟神经网络中的attention不一样)

#### Auditory Sensors and Actuators

- 内毛细胞(Inner hair cells)与外毛细胞(Outer hair cells)的不同功能
- 内毛细胞是传感器：获得basilar membrane上的信号
- 外毛细胞反作用在basilar membrane上压制一些频率

#### Theory of Filtering

- Ref. Broadbent (1958) / Treisman (1964)

#### Auditory Masking

#### Question: How to find the mask?

### Speaker extraction

#### Overview

- Ref. 1: Albert S. Bregman, Auditory Scene Analysis. Cambridge, MA, USA: MIT Press, 1990.
- Ref. 2: D. Wang, Supervised Speech Separation Based on Deep Learning: An overview, IEEE/ACM T-ASLP 2018

#### Frequency domain

- SBF-MTSAL-Concat: Ref. Optimization of speaker extraction neural network with magnitude and temporal spectrum approximation loss (ICASSP 2019).

#### Time domain

- TseNet: Ref. Time-domain speaker extraction network (ASRU 2019).
  - iVector
- SpEx: Ref. SpEx: multi-scale time-domain speaker extraction network
- SpEx+: Ref. SpEx+ (archive)

### ICASSP Paper Review

- A multi-phase gammatone filterbank for speech separation via TasNet
- Filterbank design for end-to-end speech separation
- Two-step sound separation

## 2. ICASSP2020语音识别前沿综述

- Speaker: 钱彦旻（上海交大）
- Time Stamp: 78:50

### Main Info

- Robust is still important and challenging
- Topics in E2E ASR this year
  - Streaming
  - New Models
  - General Topics

### Streaming E2E Models

- Google's E2E Research
  - 2017: 不同的端到端模型对比(LAS最优，RNNT略差)
  - 2018: E2E SOTA结果(LAS) + 性能改进(mWER、LM fusion) + 模型压缩 + Contextual ASR + Multilingual
  - 2019: On-device (RNNT) + Others (contextual, semi-supervised, multilingual, spell correction)
  - 2020: Two-Pass E2E (Interspeech19 - )
- Current challenges and existing drawbacks
  - LAS is even better than hybrid systems, but is ***difficult on streaming***
  - RNNT (ICASSP19， On-Device) is good on streaming, so the companies follow the RNNT work, including Facebook, Microsoft and Tencent, etc
  - But there is still a performance gap comparing RNNT to LAS
- References
  - A streaming on-device end-to-end model surpassing server-side conventional model quality and latency
  - Towards fast and accurate streaming end-to-end ASR
  - Streaming automatic speech recognition with the transformer model

### Adaption

- Speaker adaption
- Domain adaption
- 1 Dataset: Libri-Adapt
- References
  - Unsupervised speaker adaptation using attention-based speaker memory for end-to-end ASR
- m-vector outperforms i-vector
- Attention-based gated scaling adaptive acoustic model for CTC-based speech recognition

### Robust

- Noise-robust/far-field/accent/multi-talker/multi-channel
- MIMO-Speech: end-to-end multi-channel multi-speaker speech recognition
- Layer-normalized LSTM for hybrid-HMM and end-to-end ASR
  - BN (batch normalization)

### Other recommendations

- Machine Learning for Language Processing
  - Integrating discrete and neural features via mixed-feature trans-dimensional random field language models (Tsinghua University)
  - HOW MUCH SELF-ATTENTION DO WE NEED? TRADING ATTENTION FOR FEED-FORWARD LAYERS (RWTH)
- Confidence, Errors and OOVs
  - Speech Recognition: CONFIDENCE ESTIMATION FOR BLACK BOX AUTOMATIC SPEECH RECOGNITION SYSTEMS USING LATTICE RECURRENT NEURAL NETWORKS, Errors and OOVs (Cambridge)
  - ASR ERROR CORRECTION AND DOMAIN ADAPTATION USING MACHINE TRANSLATION (CMU)
- Representations and Embeddings
  - MOCKINGJAY: UNSUPERVISED SPEECH REPRESENTATION LEARNING WITH DEEP BIDIRECTIONAL TRANSFORMER ENCODERS (National Taiwan University)
  - WHAT DOES A NETWORK LAYER HEAR? ANALYZING HIDDEN REPRESENTATIONS OF END-TO-END ASR THROUGH SPEECH SYNTHESIS(ENCODERS (National Taiwan University)
- Word Spotting
  - INTEGRATION OF MULTI-LOOK BEAMFORMERS FOR MULTI-CHANNEL KEYWORD SPOTTING (Tencent)
  - ADAPTATION OF RNN TRANSDUCER WITH TEXT-TO-SPEECH TECHNOLOGY FOR KEYWORD SPOTTING (Microsoft)
  - TRAINING KEYWORD SPOTTERS WITH LIMITED AND SYNTHESIZED SPEECH DATA (Google)
- Large Vocabulary Continuous Speech Recognition and Search
  - MULTISTATE ENCODING WITH END-TO-END SPEECH RNN TRANSDUCER NETWORK (Google)
  - FULL-SUM DECODING FOR HYBRID HMM BASED SPEECH RECOGNITION USING LSTM LANGUAGE MODEL (RWTH)

## 3. ICASSP2020语音合成与转换前沿

- Speaker: 凌震华（中科大）& 吴志勇（清华大学）
- Time Stamp: 108:57

### 语音合成

#### 声学模型

- Seq2Seq神经网络声学模型成为绝对主流
  - Tacotron/Tacotron2
- 针对的问题
  - 稳定性
  - 韵律与表现力(引入表征韵律变化的Latent Variable, e.g. VAE/GST)
  - 个性化(重点关注目标发音人自适应数据受限场景)
  - 多语种(Code-Switching)
- Toolkit: ESPNET-TTS

#### 神经网络声码器

- 对LPCNet的改进
- WaveNet效率提升
- 非自回归的模型结构

### 语音转换

## 4. ICASSP2020基于盲源分离理论框架的语音增强前沿综述

- Speaker: 纳跃跃（阿里巴巴达摩院机器智能技术语音实验室）
- Time Stamp: 147:27

### 语音增强

- 面临的问题：远场，SNR较低
- 典型的AEC方法：NLMS(梯度下降法), RLS(最小二乘法)
- 典型的去混响方法：WPE(最小二乘法)
- 典型的声源分离方法
  - 基于波束形成的方法：输出能量最小化
    - Superdirective, DMA, MVDR, LCMV, GSC, PMWF
    - 可以归结为带限制条件的凸优化问题
  - 基于盲源分离(ICA/IVA)的方法：最大化输出信号间的独立性
    - 梯度下降法，快速不动点迭代，基于辅助函数的方法

#### 阿里做的AuxICA

- Ref. 1: [Auxiliary-function-based independent component analysis for super-Gaussian sources](https://www.researchgate.net/publication/220847982_Auxiliary-Function-Based_Independent_Component_Analysis_for_Super-Gaussian_Sources) (2010)
- Ref. 2: [Stable and fast update rules for independent vector analysis based on auxiliary function technique](https://ieeexplore.ieee.org/document/6082320) (2011 WASPAA)

#### ICASSP2020中ICA/IVA的发展

- 更快速，更稳定的迭代方法
  - Ref. [3]: Scheibler, Robin, and Nobutaka Ono: 1秩约束
  - Ref. [4]: Emura, S., et al.: L1范数约束
  - Ref. [9]: Takahashi, Yuki, et al.: 引入稀疏性假设
- Overdetermined问题/目标提取
  - Ref. [5]: Ikeshita, Rintaro, Tomohiro Nakatani, and Shoko Araki: Overdetermined，多入少出
  - Ref. [6]: Brendel, Andreas, Thomas Haubner, and Walter Kellermann, Ref. [10]: Li, Li, and Kazuhito Koishida: 引入额外信息(如空间信息)实现目标提取
  - Ref. [7]: Scheibler, Robin, and Nobutaka Ono, Ref. [8]: Jansky, Jakub, et al.: 从语音/非语音混合信号中提取主要的语音成分

#### ICASSP2020中统一语音增强框架的发展

- 去混响，波束形成联合优化，NN+信号以及纯NN的方法
  - Ref. [11]: Boeddeker, Christoph, et al., Ref. [12]: Togami, Masahito, Ref. [13]: Togami, Masahito, Ref. [14]: Nakatani, Tomohiro, et al.

#### 使用ICA框架解决回声消除问题

- ICA信号模型主要分为混合模型和分离模型
- 传统回声消除中，对近端信号没有明确建模，通常被当作误差项。所以通常还需要双讲检测或自适应迭代步长等措施。
- 基于ICA的理论框架中，近端信号建模为一个独立成分，所以其信号模型本身就具有全双工性质。

### 总结

- 盲源分离(AuxICA/IVA)除了能实现噪声抑制以外，还能用于回声消除和去混响。
- 具有统一理论框架和代码架构的音频前端。
- 全双工信号模型

### References

1. Ono, Nobutaka, and Shigeki Miyabe. "Auxiliary-function-based independent component analysis for super-Gaussian sources." International Conference on Latent Variable Analysis and Signal Separation. Springer, Berlin, Heidelberg, 2010.
2. Ono, Nobutaka. "Stable and fast update rules for independent vector analysis based on auxiliary function technique." 2011 IEEE Workshop on Applications of Signal Processing to Audio and Acoustics (WASPAA). IEEE, 2011.
3. Scheibler, Robin, and Nobutaka Ono. "Fast and Stable Blind Source Separation with Rank-1 Updates." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
4. Emura, S., et al. "A Frequency-Domain BSS Method Based on ℓ 1 Norm, Unitary Constraint, and Cayley Transform." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
5. Ikeshita, Rintaro, Tomohiro Nakatani, and Shoko Araki. "Overdetermined independent vector analysis." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
6. Brendel, Andreas, Thomas Haubner, and Walter Kellermann. "Spatially guided independent vector analysis." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
7. Scheibler, Robin, and Nobutaka Ono. "Fast independent vector extraction by iterative SINR maximization." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
8. Janský, Jakub, et al. "Adaptive blind audio source extraction supervised by dominant speaker identification using x-vectors." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
9. Takahashi, Yuki, et al. "Determined Source Separation Using the Sparsity of Impulse Responses." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
10. Li, Li, and Kazuhito Koishida. "Geometrically Constrained Independent Vector Analysis for Directional Speech Enhancement." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
11. Boeddeker, Christoph, et al. "Jointly optimal dereverberation and beamforming." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
12. Togami, Masahito. "Multi-Channel Speech Source Separation and Dereverberation With Sequential Integration of Determined and Underdetermined Models." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
13. Togami, Masahito. "Joint Training of Deep Neural Networks for Multi-Channel Dereverberation and Speech Source Separation." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.
14. Nakatani, Tomohiro, et al. "DNN-supported Mask-based Convolutional Beamforming for Simultaneous Denoising, Dereverberation, and Source Separation." ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2020.

## 5. ICASSP2020关于数据驱动的声学信号处理前沿综述

- Speaker: 张晓雷（西北工业大学）
- Time Stamp: 194:19

### 声学信号处理概览

- 语音增强、语音与声源的分离、语音的检测

### 泛化问题

- 使用unprocessed, conventional enhancers和DNN-based enhancers训练
- Sicheng Wang等提出基于teacher-student的无监督学习的方法
- Max W. Y. Lam等提出另一个基于teacher-student的无监督学习的方法
- Yuma Koizumi等提出了一个基于不同说话人的语音增强自适应方法
  - Ref. Speech enhancement using self-adaptation and multi-head self-attention

### 实时性问题

- TasNet时延短(Ref. Conv-TasNet)
- 研究TasNet的loss
  - Ref. Demystifying tasnet: a dissecting approach
- 研究一维卷积与人工设计特征的效果
  - Ref. A multi-phase gammatone filterbank for speech separation via tasnet
- Analytic filter

### 远场问题

- 目前以固定阵列的多通道搭音为主
- 基于深度学习的方法可以分为两类：(1)空间特征法 (2)基于深度学习的空间滤波器
- 自组织阵列
  - Ref. Deep ad-hoc beamforming
- 通道加权
  - 基于多通道的DANSE方法
  - FasNet + TAC
  - 2 stages: 先多通道训练，后分开

### 多模态问题

- 音视频语音增强
  - 避免视觉信息干扰
  - 已有的方法往往只用到了一个人说话人的信息，Yanmin Qian提出充分利用视觉信息（多个人）
  - 唇动特征、面部特征

## 6. ICASSP2020基于深度学习的说话人识别及说话人日志前沿综述

- Speaker: 李明（昆山杜克大学）
- Time Stamp: 171:22

### 基于深度学习的说话人识别

- 特征
- 复杂场景
  - 超短时的文本无关声纹识别: Ref. Frame-Level Phoneme Invariant Speaker Embedding for Text-Independent Speaker Recognition on Extremely Short Utterances
    - 应用在multi-filter, diarization
- 数据库
  - Ref. CN-CELEB: A Challenging Chinese Speaker Recognition Dataset
    - 作者说CN-Celeb比VoxCeleb数据更接近真实场景
  - Ref. HI-MIA: A Far-Field Text-Dependent Speaker Verification Database and the Baselines
- 编码层
- 网络结构
  - Ref. Frequency and Temporal Convolutional Attention for Text-Independent Speaker Recognition
  - ...
  - Ref. H-VECTORS: Utterance-Level Speaker Embedding Using A Hierarchical Attention Model
  - 声纹与唤醒share层：Ref. Multi-Task Learning for Speaker Verification and Voice Trigger Detection
- 领域自适应

## 7. DCASE声音场景与事件检测分类挑战赛前沿综述

- Speaker: 李圣辰（北京邮电大学）
- Time Stamp: 224:15

### 声学信号处理典型任务

- 声学场景检测
- 声学事件标记
- 声学事件检测

### 数据库的构建

- DCASE
- CHiME-HOME
- AudioSet，谷歌维护
- 其他数据库

### 常用解决方法

- 频谱遮盖+模板匹配
- 特征提取+分类器
  - 输入多采用梅尔相关特征或其他时频特征
  - 非语义特征提取采用CRNN结构
  - 分类器及其融合

### 声学信号处理的挑战

- 计算效率
  - 算法压缩
    - SVD分解
    - 神经网络剪枝
    - 权重数值表示量化或二值化
    - 知识蒸馏与任务分解
- 标签与数据库
- 识别率问题
- 实用性问题
  - 快速识别声学场景
  - 识别不同领域的声学场景
  - 不同声学采集设备的兼容问题
  - 嘈杂环境中的声学事件识别

### 总结

- 基于机器学习方法，可以实现声学信号的识别
- 基于声学信号识别，可以实现人工智能的应用
- 尽管取得了一定的进步，识别率仍需要提高，语义描述系统待完善
- 在实际应用过程中，还有计算效率优化的问题和算法适应性的问题
