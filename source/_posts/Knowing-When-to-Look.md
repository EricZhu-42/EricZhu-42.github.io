---
title: Knowing When to Look - Adaptive Attention via A Visual Sentinel for Image Captioning
date: 2019-08-10
tags: [machine learning]
mathjax: true
---

## 论文信息

| 类别     | 内容                                                         |
| -------- | ------------------------------------------------------------ |
| 论文名称 | [Knowing When to Look: Adaptive Attention via A Visual Sentinel for Image Captioning](http://arxiv.org/abs/1612.01887) |
| 发表时间 | CVPR 2017                                                    |
| 作者     | [Jiasen Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C+J), [Caiming Xiong](https://arxiv.org/search/cs?searchtype=author&query=Xiong%2C+C), [Devi Parikh](https://arxiv.org/search/cs?searchtype=author&query=Parikh%2C+D), [Richard Socher](https://arxiv.org/search/cs?searchtype=author&query=Socher%2C+R) |
| 关键词   | Image Captioning, Attention Mechanism                        |

## 核心信息

### 文章领域及方向

CV & NLP，Image Captioning 问题

### 文章概要

本文提出了Joint Inference与Context Fusion这两种新方法来解决在Dense Captioning任务中出现的问题。这两种方法利用图片的整体环境信息与物体周围的局部信息，生成对感兴趣区域（Region of Interest, ROI）的描述与检测框的偏移值，在综合得分（mean Average Precision, mAP）指标上达到了当时的state-of-the-art水平。

<!-- more --> 

## 引言

### 当前主流方案

当前主要使用的方案是基于视觉Attention的**Encoder-Decoder**模型，其中Attention机制会指出在每个词语生成过程中**语义对应的图像区域**。

### 当前方案存在的问题

并不是Caption中的**所有**单词都有对应的视觉特征。例如，在句子 "A white bird perched on top of a red stop sign." 中，单词 "A", "of" 就**不存在**对应的视觉特征。此外，语言本身的一些特征用法，如 "perched" 后紧跟的 "on" 和 "top", 和 "a red stop" 后紧跟的 "sign", 使得在生成部分单词时是**不需要参考**对应的视觉特征的。

事实上，**非视觉单词**的梯度会误导模型，并且在Caption过程中降低整个视觉特征的有效性。

### 本文贡献

本文提出了一种基于**自适应性（Adaptive）Attention**的Encoder-Decoder框架结构。它可以自动地决定**何时应当参考视觉特征**，而**何时应当仅依赖于语言模型**。当然，在模型参考视觉特征时，它也会决定应当关注图片的哪个区域。

首先，我们提出了一种创新的**spatial Attention**模型来提取图片中的空间特征。此外，在提出**自适应性Attention机制**的同时，我们引入了一种**新的LSTM结构**，即LSTM层在生成hidden state的同时，会生成一个额外的 visual sentinel（**视觉哨兵**）。视觉哨兵保存了Decoder的记忆。在生成新词时，模型会通过一个**sentinel gate**来决定从图片中获得多少信息，即生成该词是应当参考视觉特征亦或是依赖于语言模型。

总的来说，本文贡献如下：

1. 提出了一种基于**自适应性（Adaptive）Attention**的Encoder-Decoder框架结构。它可以自动地决定**何时应当参考视觉特征**，而**何时应当仅依赖于语言模型**。
2. 提出了创新的**spatial Attention**机制来提取图片中的空间特征。
3. 提出了LSTM的一种新的拓展，即通过在hidden state外新增一个**visual sentinel**来解决单词的生成过程对图像特征的依赖程度不固定的问题。

## 具体方法

### Encoder-Decoder结构

对于给定的图像与对应的Caption文本，Encoder-Decoder结构直接优化如下的目标函数：
$$
\theta^* = arg\ max\ \theta \sum_{(I,y)}log\ p(y|I;\theta)
$$
 其中 $\theta$ 为模型的参数，$I$ 为图像，$y=\lbrace y_1, \dots,\ y_n \rbrace$ 为对应的Caption文本。

根据链式法则，联合概率分布的对数似然可以被分解成如下的有序条件概率：
$$
log\ p(y) = \sum_{t=1}^T log\ p(y_t|y_1, \dots,y_{t-1},I)
$$
为了方便起见，我们此处暂时不考虑对模型参数的依赖关系。

在Encoder-Decoder结构中，每个词语的条件概率可以被表示为：
$$
log\ p(y_t|y_1,\dots,y_{t-1},I) = f(h_t,c_t)
$$
其中 $f$ 是输出 $y_t$ 概率的一个非线性函数，$c_t$ 是在时刻 $t$ 从图像 $I$ 中提取出的视觉特征向量，$h_t$ 是时刻 $t$ RNN的hidden-state. 

我们在本文中采用LSTM作为RNN的实际模型。对于LSTM，$h_t$ 可以被如下表示：
$$
h_t = LSTM(x_t,h_{t-1},m_{t-1})
$$
其中 $x_t$ 为输入向量，$m_{t-1}$ 是在 $t-1$ 时刻的记忆单元。

