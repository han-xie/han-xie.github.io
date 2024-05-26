---
layout: post
title: 使用kaldi训练aishell数据集
tags: [tech]
---

<h2 id="contents"> 目录 </h2>

1. [数据准备](#preparation)
2. [提取特征](#feature)
3. [单音素训练](#monophone)
4. [三音素训练](#triphone)
5. [nnet3网络训练](#nnet3)
6. [chain网络训练](#chain)

<h2 id="preparation"> 1. 数据准备 </h2>

数据准备分为六个步骤，所用到的脚本列表如下：

步骤 | 脚本
---|---
1. 下载数据 | local/download_and_untar.sh
2. 准备dict | local/aishell_prepare_dict.sh
3. 准备语音 | local/aishell_data_prep.sh
4. 准备lang | utils/prepare_lang.sh
5. 训练语言模型 | local/aishell_train_lms.sh
6. 生成语言模型fst | utils/format_lm.sh

#### 下载数据

- 脚本：local/download_and_untar.sh
- 输入：下载地址和存放路径
- 输出：下载到的音频和resource_aishell资源文件

#### 准备dict

- 脚本：local/aishell_prepare_dict.sh
- 输入：资源文件路径
- 输出：lexicon.txt, nonsilence_phones.txt, optional_silence.txt, 问题集extra_questions.txt
- 其他信息：aishell问题集的设计：简单得将音调相同的归类(TBD)

#### 准备语音

- 脚本：local/aishell_data_prep.sh
- 输入：语音和标注文件路径
- 输出：(1) 在训练、验证和测试集文件夹中生成wav.scp, utt2spk, spk2utt (2) 各子集的transcripts.txt排序后得到text
- 默认路径是data/local/train, data/local/dev, data/local/test以及data/train, data/dev, data/test

#### 准备lang

- 脚本：utils/prepare_lang.sh
- 输入：**音素是否位置相关**, dict, \<SPOKEN_NOISE\>
- 输出：(1) data/local/lang: lexicon\*,  align_lexicon.txt, phone_map.txt (2) data/lang: phones, oov, topo, words.txt, L.fst

#### 训练语言模型

- 脚本：local/aishell_train_lms.sh
- 输入：data/local/train/text和data/local/dict/lexicon.txt
- 输出：主要是data/local/lm/3gram-mincount/lm_unpruned.gz，中间有text.no_oov, word.counts, unigram.counts, word_map等
- 其他信息
  - 语言模型训练脚本在\<kaldi_root\>/tools/kaldi_lm/train_lm.sh
  - **支持的语言模型类型**有3gram, 3gram-mincount, 4gram, 4gram-mincount
  - 生成unigram.counts时有**两个小问题**: (1) 去SIL (2) count应该只+1。估计对最终结果影响不大。
  - word_map将\<s\>, \</s\>, \<SPOKEN_NOISE\>和所有词map到A, B, C, D...

#### 生成语言模型fst

- 脚本：utils/format_lm.sh
- 输入：前面的data/lang，arpa格式语言模型，lexicon.txt
- 输出：data/lang_test/G.fst
  - 其他文件都是从data/lang复制过来的，包括phones.txt words.txt topo L.fst L_disambig.fst phones oov.int oov.txt
- 这一步主要用到了arpa2fst将语言模型进行**格式转换**，并且对生成的G.fst做了一些验证，例如stochastic，有没有不输出词的路径

[返回目录](#contents)

<h2 id="feature"> 2. 提取特征 </h2>

#### 提取mfcc_pitch特征

- 脚本：steps/make_mfcc_pitch.sh，这个脚本是结合了make_mfcc.sh和make_pitch_kaldi.sh
- 输入：(1) 训练、验证和测试文件夹路径, 如data/train（需要有wav.scp）(2) 特征配置文件: mfcc.conf和pitch.conf
- 输出：计算出的特征（可以自行指定路径）以及feats.scp（默认在data/train等文件夹里）
- 其他信息
  - 特征维度为16维，用feat-to-dim可以查看
  - 这里可以选择是否支持VTLN：特征级声道长度归一（Feature-level Vocal Tract Length Normalization）
  - \[info\]: no **segments** file exists: assuming wav.scp indexed by utterance. 这是正常的，segments是指定音频端点信息的文件
  - 这里可以多核并行计算，最终的feats.scp也是把多个核的合并起来的

#### 计算CMVN(Cepstral Mean and Variance Normalization)：倒谱均值和方差归一化

- 脚本：steps/compute_cmvn_stats.sh
- 输入：训练、验证和测试文件夹路径, 如data/train
- 输出：基于per speaker的CMVN（可以自行指定路径）以及cmvn.scp（默认在data/train等文件夹里）
- 因为后面要做基于per speaker的fMLLR，对每一条语音做CMVN没有意义。\<TBD\>因为损失了说话人信息？？？\</TBD\>

#### 验证文件夹里的数据

- 脚本：utils/fix_data_dir.sh
- 输入：训练、验证和测试文件夹路径, 如data/train
- 输出：(1) 验证了文件是按第一列排过序并且都是第一列是unique的，即```sort -k1,1 -u``` (2) 取了feats.scp, cmvn.scp, wav.scp, text, utt2spk等文件中都存在的交集部分

[返回目录](#contents)

<h2 id="monophone"> 3. 单音素训练 </h2>

### 训练mono

- 输入：data/train, data/lang，模型输出到exp/mono
- 感觉重要的参数：max_iter, 各种beam, realign的stage

训练步骤：（[参考许开拓的博文](https://blog.csdn.net/u010731824/article/details/69668765)）

1. 初始化单音素模型。调用gmm-init-mono，生成0.mdl、tree。
2. 编译训练时的图。调用compile-train-graph生成text中每句抄本对应的fst，存放在fsts.JOB.gz中。
3. 第一次对齐数据。调用align-equal-stats-ali生成对齐状态序列，通过管道传递给gmm-acc-stats-ali，得到更新参数时用到的统计量。
4. 第一次更新模型参数。调用gmm-est更新模型参数。
5. 进入训练模型的主循环：在**指定的对齐轮数**，使用gmm-align-compiled对齐特征数据，得到新的对齐状态序列；每一轮都调用gmm-acc-stats-ali计算更新模型参数所用到的统计量，然后调用gmm-est更新模型参数，并且在每一轮中增加GMM的分量个数。

### 解码mono

#### 1. 生成graph

- 脚本：utils/mkgraph.sh
- 输入：L.fst G.fst phones.txt words.txt phones/silence.csl phones/disambig.int model tree
- 输出：graph/HCLG.fst

#### 2. 执行解码

- 脚本：steps/decode.sh
- 输入：前面的graph, data/dev或data/test文件夹，decode.config
- 输出：CER, WER, lattice到mono/graph/decode_dev和decode_test文件夹
- ***注意：解码脚本根据文件夹里是否有final.mat判断是delta feature还是LDA feature***

### 对齐mono到mono_ali

- 脚本：steps/align_si.sh
- 输入：train的text, oov.int, 上一步的tree和final.mdl

对齐步骤：

1. 检查一下训练时的phones.txt只是比最开始lang里的多了消歧音素
2. 调用compile-train-graph生成对应的fst
3. 使用gmm-align-compiled对齐特征数据
4. 执行steps/diagnostic/analyze_alighments.sh验证

对齐的结果可以用```show-alignments```指令查看: ([参考链接](http://kaldi-asr.org/doc/tutorial_running.html))

```bash
$ show-alignments phones.txt final.mdl "ark:gunzip -c ali.1.gz |" | less
```

[返回目录](#contents)

<h2 id="triphone"> 4. 三音素训练 </h2>

aishell的三音素训练有5个步骤：tri1, tri2, tri3a, tri4a和tri5a

步骤 | feature<sup>[1]</sup> | 训练脚本 | 解码脚本 | 对齐脚本
---|---|---|---|---
tri1 | delta | steps/train_deltas.sh | steps/decode.sh | steps/align_si.sh
tri2 | delta | steps/train_deltas.sh | steps/decode.sh | steps/align_si.sh
tri3a | LDA | steps/train_lda_mllt.sh | steps/decode.sh | steps/align_fmllr.sh
tri4a | LDA | steps/train_sat.sh | steps/decode_fmllr.sh | steps/align_fmllr.sh
tri5a | LDA | steps/train_sat.sh | steps/decode_fmllr.sh | steps/align_fmllr.sh

[1] 这列不是很确定，delta与LDA的区别需要再研究一下

### 训练tri1

- 训练脚本：steps/train_deltas.sh
  - 输入：叶子节点数，总的gaussian数，data/train，data/lang, exp/mono_ali
  - 输出：exp/tri1, 主要是final.mdl和tree
- 解码脚本与mono相同
- 对齐脚本与mono相同

### 训练tri2

- 所有脚本与tri1完全一样，迭代了一次。

### 训练tri3a: LDA+MLLT

- 训练脚本：steps/train_lda_mllt.sh
  - 输入：叶子节点数，总的gaussian数，data/train，data/lang, exp/tri2_ali
  - 输出：exp/tri3a，主要是final.mdl和tree
- 解码脚本跟mono一样
- ***这一步开始对齐脚本有变化***

### 训练tri4a和tri5a

- 训练脚本：steps/train_sat.sh
  - tri5a比tri4a用的gauss节点更多
- ***这一步开始解码脚本也有了变化***

[返回目录](#contents)

<h2 id="nnet3"> 5. nnet3网络训练 </h2>

### 提取特征：ivector和mfcc_hires

这一步的总脚本为local/nnet3/run_ivector_common.sh，其中包含以下步骤：

1. utils/data/perturb_data_dir_speed_3way.sh用于准备data/train_sp（speed-perturbed data）
2. steps/align_fmllr.sh用于将perturbed data对齐（结果在tri5a_sp_ali）
3. steps/make_mfcc_pitch.sh用于提取mfcc_hires特征，其配置文件为conf/mfcc_hires.conf，在用这个脚本提取mfcc_hires特征之前用utils/data/perturb_data_dir_volume.sh脚本做了volume perturbation
4. 分离出没有pitch的mfcc_hires特征用于extract iVector：utils/data/limit_feature_dim.sh
5. steps/online/nnet2/get_pca_transform.sh计算pca
6. steps/online/nnet2/train_diag_ubm.sh训练diagonal UBM
7. steps/online/nnet2/train_ivector_extractor.sh训练iVector extractor (结果放在exp/nnet3/extractor)
8. steps/online/nnet2/extract_ivectors_online.sh用于extract iVector，对于验证集和测试集没有用speed perturbation

### 训练nnet3神经网络

主体脚本在local/nnet3/run_tdnn.sh里，其主要步骤如下：

1. 生成神经网络配置文件：输入层为mfcc_hires特征，维度为43。输出层维度为gmm probability density function的个数。
2. 调用steps/nnet3/train_dnn.py训练神经网络
3. 解码验证集和测试集，解码用的graph是tri5a的graph

Tips: 在local/nnet3/tuning文件夹下面还有一个run_tdnn_2a.sh脚本，其中增加了online decoding。

[返回目录](#contents)

<h2 id="chain"> 6. chain网络训练 </h2>

- 对齐的脚本跟nnet3用得不一样，用的是align_fmllr_lats.sh，这个脚本会生成对齐的lattice
- chain训练的时候需要自行生成tree，脚本是steps/nnet3/chain/build_tree.sh
- 调用steps/nnet3/chain/train.py训练神经网络
- 与nnet3不同的是这里先调用utils/mkgraph.sh生成了graph然后解码

[返回目录](#contents)
