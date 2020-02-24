---
layout: post
title: 使用kaldi训练aishell数据集
tags: [asr]
---

<h2 id="contents"> 目录 </h2>
1. [数据准备](#preparation)
2. [提取特征](#feature)
3. [单音素训练](#mono)
4. [三音素训练1](#tri1)
5. [三音素训练2](#tri2)
6. [三音素训练3a](#tri3a)
7. [三音素训练4a](#tri4a)
8. [三音素训练5a](#tri5a)
9. [nnet3网络训练](#nnet3)
10. [chain网络训练](#chain)

<h2 id="preparation"> 1. 数据准备 </h2>
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
- 输出：G.fst
- 其他文件都是从data/lang复制过来的，包括phones.txt words.txt topo L.fst L_disambig.fst phones oov.int oov.txt
- 这一步主要用到了arpa2fst将语言模型进行**格式转换**，并且对生成的G.fst做了一些验证，例如stochastic，有没有不输出词的路径


[返回目录](#contents)

<h2 id="feature"> 2. 提取特征 </h2>

[返回目录](#contents)

<h2 id="mono"> 3. 单音素训练 </h2>

[返回目录](#contents)

<h2 id="tri1"> 4. 三音素训练1 </h2>

[返回目录](#contents)

<h2 id="tri2"> 5. 三音素训练2 </h2>

[返回目录](#contents)

<h2 id="tri3a"> 6. 三音素训练3a </h2>

[返回目录](#contents)

<h2 id="tri4a"> 7. 三音素训练4a </h2>

[返回目录](#contents)

<h2 id="tri5a"> 8. 三音素训练5a </h2>

[返回目录](#contents)

<h2 id="nnet3"> 9. nnet3网络训练 </h2>

[返回目录](#contents)

<h2 id="chain"> 10. chain网络训练 </h2>

[返回目录](#contents)

