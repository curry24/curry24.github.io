---
layout:     post
title:      Attention
category: blog
description: 对于Sequence to Sequence中Attention的一些理解
---

## AI challenger

由于参加了AI challenger中的image caption任务的比赛，借此机会，看了一些涉及Attention的论文，进一步加深了对于Attention机制的了解。


## Attention in image caption

首先是第一篇论文[Show, Attend and Tell: Neural Image Caption Generation with Visual Attention][1]。这篇论文首次在 image caption 中使用了 attention 机制。与原始的论文[Show and Tell: A Neural Image Caption Generator][2]中不同的是，原始的im2txt 将图片转化为一个向量，然后再使用全连接层将该图片向量转化为与词向量相同维度的向量。
而在添加了 attention 的 im2txt 中，提取到的是图片卷积之后的一个矩阵特征映射。例如，在论文中，将图片提取位 14x14 的矩阵，而矩阵中的每一块都是一个与词向量维度相同的向量。因此 attention 的目的训练矩阵中每一块映射与 decoder 出的每一个词的关系。

### Soft attention 和 Hard attention

本篇论文中，提出了两种 attention 的方式，分别是 soft attention 以及hard attention。大致区别如下：soft attention 在计算权重的时候使用了图片矩阵中每一块，即图片矩阵中每一块与当前生成词都有一个权重大小。而 hard attention 的实现也相对更加 hard，在计算权重时，hard attention 仅仅使用图片矩阵中与生成词最密切的那些矩阵块来进行计算。这一过程更加复杂，但是也更精确。最终的结果也表明，hard attention 的效果要比 soft attention 的效果要好。

## Attention in NMT

接下来是第二篇论文[Effective Approaches to Attention-based Neural Machine Translation][3]。这篇论文在原始的基于 attention 的 NMT 的论文[Neural machine translation by jointly learning to align and translate][4]的基础上进行了进一步地改进和加强，并且借鉴了本文中提到的第一篇论文中的soft and hard attention的思想，提出了 global attention 以及 local attention 两种 attention 的模型。

### Global attention
在计算 context vector 的时候，要考虑 encoder 过程中的所有的 hidden states，即把 decoder 过程中当前的 hidden state vector 与 encoder 中所有的 hidden state vector 来进行处理，从而可以得到 context vector，最后再将 context vector 与 decoder 中初始的 hidden state 进行结合处理，从而得到新的 hidden state。

### Local attention
与 Global attention 不同的是，Local attention 在计算 context vector的时候，仅仅考虑 encoder 中窗口大小下的 hidden states。窗口大小是根据经验来选择的。并且论文中也介绍了两种不同的窗口位置选择方式，local-m以及 local-p。local-m 粗略地认为原语言与目标语言是单调对齐的；local-p则是通过一个公式来计算窗口的位置。当然，第二种方式最终的表现更好。


[1]: https://arxiv.org/pdf/1502.03044
[2]: https://arxiv.org/pdf/1411.4555.pdf
[3]: https://arxiv.org/pdf/1508.04025.pdf
[4]: https://arxiv.org/pdf/1409.0473
