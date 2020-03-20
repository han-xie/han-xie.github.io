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

## Discriminative Training

### Discriminative Training for Large Vocabulary Speech Recognition (2003)

- Author: Daniel Povey (Ph.D. thesis submitted to the University of Cambridge)
- A discriminative criterion called Minimum Phone Error is introduced in this thesis.
- 1986, Maximum Mutual Information (MMI, also known as Maximum Conditional Likelihood), 1993, 1997, Minimum Classification Error (MCE), Minimum Phone Error (MPE)
- Bayes' rule (p.5, Fig. 2.2): **prior probability**, **posterior probability**
- Early HMMs use discrete output symbols, which were obtained by so-called "**Vector Quantization**". At the moment essentially all work on speech recognition uses **continuous vector-valued output symbols**. (p. 8)
- **History of speech recognition**: 1968 (Dynamic Time Warping), 1975 (HMM, Dragon system), 1985 (GMM-HMM), 1985 (Replace whole-word models with phone models and context-dependent phone models), 1994 (Maximum A Posterior) & 1995, 1996 (Maximum Likelihood Linear Regression) for speaker adaption, 1994 (phone clustering), 1996, 1999 (Vocal Tract Length Normalization), 1998, 2000 (Linear Discriminant Analysis). Apart from the mainstream techniques, other directions of research have found use in **small vocabulary systems**.

#### Objective functions for training HMMs

- Standard objective function used in Maximum Likelihood Estimation

`$$
\mathcal{F}_{ \text{MLE} }(\lambda) =
\displaystyle\sum_{r=1}^{R} \log p_\lambda \left( \mathcal{O}_r | s_r \right),
$$`
where `$s_r$` is the correct transcription of the *r*-th speech file `$\mathcal{O}_r$`. `$\lambda$` denotes all the parameters of a set of HMMs.

- Maximum Mutual Information objective function

`$$
\mathcal{F}_{ \text{MMI} }(\lambda) =
\displaystyle\sum_{r=1}^{R} \log \frac{p_\lambda \left( \mathcal{O}_r | s_r \right)^\kappa P\left( s_r \right)^\kappa}{\sum_s p_\lambda \left( \mathcal{O}_r | s \right)^\kappa P\left( s \right)^\kappa},
$$`
where `$P(s)$` is the language probability for sentence `$s$`.

- Minimum Classification Error ojbective function

#### Why we need to use discriminative objective functions?

- The standard objective function has a strong assumption. Observation at time *t* depends on the corresponding hidden state only. This is not exactly true [[Reference](https://medium.com/@jonathan_hui/speech-recognition-maximum-mutual-information-estimation-mmie-a0db565764aa)].