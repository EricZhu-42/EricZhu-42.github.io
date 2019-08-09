---
title: Show, Attend and Tell - Neural Image Caption Generation with Visual Attention
date: 2019-08-08
tags: [machine learning]
---

## 论文信息

| 类别     | 内容                                                         |
| -------- | ------------------------------------------------------------ |
| 论文名称 | [Show, Attend and Tell: Neural Image Caption Generation with Visual Attention](https://arxiv.org/abs/1502.03044) |
| 发表时间 | ICML 2015                                                    |
| 作者     | [Kelvin Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C+K), [Jimmy Ba](https://arxiv.org/search/cs?searchtype=author&query=Ba%2C+J), [Ryan Kiros](https://arxiv.org/search/cs?searchtype=author&query=Kiros%2C+R), [Kyunghyun Cho](https://arxiv.org/search/cs?searchtype=author&query=Cho%2C+K), [Aaron Courville](https://arxiv.org/search/cs?searchtype=author&query=Courville%2C+A), [Ruslan Salakhutdinov](https://arxiv.org/search/cs?searchtype=author&query=Salakhutdinov%2C+R), [Richard Zemel](https://arxiv.org/search/cs?searchtype=author&query=Zemel%2C+R), [Yoshua Bengio](https://arxiv.org/search/cs?searchtype=author&query=Bengio%2C+Y) |
| 关键词   | Image Captioning, Attention Mechanism                        |

## 核心信息

### 文章领域及方向

CV & NLP，Image Captioning 问题

### 文章概要

本文提出了Joint Inference与Context Fusion这两种新方法来解决在Dense Captioning任务中出现的问题。这两种方法利用图片的整体环境信息与物体周围的局部信息，生成对感兴趣区域（Region of Interest, ROI）的描述与检测框的偏移值，在综合得分（mean Average Precision, mAP）指标上达到了当时的state-of-the-art水平。

<!-- more --> 

## 引言

### Attention的特性

1. Attention使得模型能够在需要的时候**自动地**关注于图片上的**某个显著对象**。
2. 本文中的Attention有两种主要类别：**Hard Attention**与**Soft Attention**。
3. Attention能够协助可视化**模型究竟“看到了什么”**。

### 本文的贡献

1. 介绍了在统一框架下的两种Attention机制：**Soft Attention**（通过标准的反向传播方法进行训练）与**Hard Attention**（通过最大化近似变分下界进行训练）。