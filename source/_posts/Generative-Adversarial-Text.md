---
title: Generative Adversarial Text to Image Synthesis
date: 2019-08-06
tags: [machine learning]
---

## 论文信息

| 类别     | 内容                                                         |
| -------- | ------------------------------------------------------------ |
| 论文名称 | [Generative Adversarial Text to Image Synthesis](https://arxiv.org/abs/1605.05396) |
| 发表时间 | ICML 2016                                                    |
| 作者     | [Scott Reed](https://arxiv.org/search/cs?searchtype=author&query=Reed%2C+S), [Zeynep Akata](https://arxiv.org/search/cs?searchtype=author&query=Akata%2C+Z), [Xinchen Yan](https://arxiv.org/search/cs?searchtype=author&query=Yan%2C+X), [Lajanugen Logeswaran](https://arxiv.org/search/cs?searchtype=author&query=Logeswaran%2C+L), [Bernt Schiele](https://arxiv.org/search/cs?searchtype=author&query=Schiele%2C+B), [Honglak Lee](https://arxiv.org/search/cs?searchtype=author&query=Lee%2C+H) |
| 关键词   | Text2Image, GAN                                              |

## 核心信息

### 文章领域及方向

CV & NLP，Text2Image 问题

### 文章概要

本文提出了Joint Inference与Context Fusion这两种新方法来解决在Dense Captioning任务中出现的问题。这两种方法利用图片的整体环境信息与物体周围的局部信息，生成对感兴趣区域（Region of Interest, ROI）的描述与检测框的偏移值，在综合得分（mean Average Precision, mAP）指标上达到了当时的state-of-the-art水平。

<!-- more --> 

## 引言

### 解决该任务的两个关键点

1. 学习对**图片中重要细节**的**特征表达文本**
2. 使用特征来**生成**足够欺骗人类的**图片**

### 当前的深度神经网络存在的问题

图像-文本描述的组合是**多模式**的，即：

1. 对于**同一段**文本描述，有**极为大量的**图片可以合理地与它对应

2. 对于**同一张**图片，有**较多的**文本描述可以合理地与它对应（可以通过将文本序列分解的方法解决）

**GAN**是解决这些问题的有效方法之一。GAN中的**生成模型**（Generator）可以对每段描述生成一张对应图片（不考虑随机噪声），再用**判别模型**（Discriminator）对其进行判定与优化。此时，判别模型便承担了“损失函数”的角色。

### 本文的主要贡献

构建了一个？？？

### 相关工作