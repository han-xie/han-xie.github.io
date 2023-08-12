---
layout: post
title: 【笔记】命令行文本处理工具记录
tags: [note, linux]
---

## 目录

1. [背景介绍](#introduction)
2. [awk的使用](#awk)

<h2 id="introduction"> 1. 背景 </h2>

在命令行处理文本数据是程序员或者数据工程师的基本技能，学会使用一些常用的工具对于提升效率有很大帮助。这里记录了一些常用工具的使用方法，以便以后查阅。

<h2 id="awk"> 2. awk的使用 </h2>

菜鸟编程上关于awk的使用介绍链接如下：<https://www.runoob.com/linux/linux-comm-awk.html>

<h3> 2.1 处理你好问问数据中的应用 </h3>

在处理[你好问问唤醒数据](https://openslr.org/87/)时，使用到awk处理数据的代码如下：
```bash
  for file in label_orig_sorted.txt transcript_orig_sorted.txt \
    utt2spk_orig_sorted wav_orig_sorted.scp; do
    awk -v new_utt_id=$dir/utt_new.list \
      'BEGIN {
         while (getline < new_utt_id) {
           new_id[$1] = $2
         }
       }
       {
         printf("%s", new_id[$1]);
         for(i=2;i<=NF;i++) printf(" %s", $i);
         printf("\n")
       }' $dir/$file > $dir/${file/orig_sorted/new}
    sort -u $dir/${file/orig_sorted/new} > $dir/${file/_orig_sorted/}
  done
```

<h3> 2.2 学习使用awk的脚本示例 </h3>

我在github上保存了学习使用awk的示例脚本：[参考链接](https://github.com/han-xie/code_notes/blob/master/shell/02_awk.sh)。
