---
layout: post
title: Literature review notes on automatic speech recognition
tags: [asr]
---

## Deep Neural Network

### Supervised Sequence Labelling with RNN (Graves)

- Ch. 2: Supervised Learning, Supervised Pattern Classification, Supervised Sequence Labelling
  - Pattern Classification (Recognition): Multilayer Perceptrons, Support Vector Machines.

## Token Passing Model

For me, I find it necessary to know how the Viterbi algorithm works in order to understand this token passing model. A good explanation in Chinese is available [here](https://www.zhihu.com/question/20136144).

### Token Passing: a Simple Conceptual Model for Connected Speech Recognition Systems (1989)

- Authors: S.J. Young, N.H. Russell, J.H.S Thornton
- Introduction
  - **One Pass** and **Level Building** algorithms were essentially idential when syntax contraints were applied
- **The central point of this paper** is that token passing model leads to much simpler and more powerful generalization than lattice model. (p. 3)
- In order to understand the algorithm, readers need to keep in mind that tropical semiring is usually used in speech recognition. In tropical semiring, value $0$ denotes $\bar{1}$ and value $\infty$ denotes $\bar{0}$ [[Reference](https://x-algo.cn/index.php/2017/04/29/speech-recognition-decoder-1-automata-and-semi-circular/)].
- DTW is effectively a special case of HMM recognition.
- n-best token (p. 14)
- Conclusion: the advantage of Token Passing Model is that it has a clean interface and it is very straightforward.
