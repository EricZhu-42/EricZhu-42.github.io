---
title: Actor-Critic Sequence Training for Image Captioning
date: 2019-08-03
tags: [machine learning]
mathjax: true
---

## 论文信息

| 类别     | 内容                                                         |
| -------- | ------------------------------------------------------------ |
| 论文名称 | [Actor-Critic Sequence Training for Image Captioning](https://arxiv.org/abs/1706.09601) |
| 发表时间 | CVPR 2017                                                    |
| 作者     | [Li Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C+L), [Flood Sung](https://arxiv.org/search/cs?searchtype=author&query=Sung%2C+F), [Feng Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C+F), [Tao Xiang](https://arxiv.org/search/cs?searchtype=author&query=Xiang%2C+T), [Shaogang Gong](https://arxiv.org/search/cs?searchtype=author&query=Gong%2C+S), [Yongxin Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C+Y), [Timothy M. Hospedales](https://arxiv.org/search/cs?searchtype=author&query=Hospedales%2C+T+M) |
| 关键词   | Image Captioning, Reinforcenemt Learning                     |

## 核心信息

### 文章领域及方向

CV & NLP & RL，Image Captioning 问题

### 文章概要

本文通过构造Actor-Critic结构，利用强化学习解决了Encoder-Decoder结构在Image Captioning任务上存在的两个问题。本文采用Actor网络选择生成的词语序列，用Critic网络对Actor的决策加以评价，通过训练对Actor-Critic进行优化，最终在CIDERr等指标上达到了较高水平。

<!-- more --> 

## 引言

### 本文重点

通过强化学习（Reinforcement Learning, RL）方法训练出在Captioning任务中更好的**语言生成模型**（Decoder），而不是**对图片的理解模型**（Encoder）。

### 当前Encoder-Decoder模型存在的问题

1. 因为模型在训练阶段**不直接接触自己生成的文本描述**，故在测试阶段由于样本不完全匹配，会导致**误差的累积**。
2. 模型训练常常采用**交叉熵**（**可微**）而不是CIDEr等**实际反映生成语言质量的指标**（**不可微**）作为损失函数。在训练阶段最优化交叉熵常常**并不能**最优化CIDEr等指标，这造成了训练的**偏差**与**低效**。

### 针对上述两个问题提出的解决方案

1. 通过**在训练过程中对模型的reward进行采样并优化**，解决训练阶段与测试阶段的不匹配问题。
2. 通过**将CIDEr等相关指标直接作为reward**并进行优化的方法，解决损失函数不高效的问题。

### 模型设计思路

本文通过采用**Actor-Critic**模型来解决Image Captioning问题。模型由如下两个部分组成：

1. **策略网络**（actor）：将Captioning视为给定图像的**序列决策**问题，根据图像**预测对应的Caption**（决策行为对应于单词的选择过程）。
2. **价值网络**（critic）：对于每一个**状态**（包含图像与当前已经生成的部分序列），根据语言指标（如CIDEr）**预测当前状态的value**（对于尚未生成的序列部分，基于当前的概率分布对其继续采样至序列完整）。

**价值网络（critic）预测得到的value将被用来训练策略网络（actor）**。假设actor预测的value值是准确的，那么actor使用value进行训练时将不会产生上述问题中提到的偏差。

与大多数强化学习任务相比，Image Captioning任务中**动作的数量更多**（如词典内词语的数量超过10K），但**训练的周期更短**（每一个序列生成完毕即结束）。

### 相关工作

1. **Image Captioning**：当前主要采用Encoder-Decoder模型，其中Encoder主要采用CNN，Decoder主要采用RNN。模型的细节在上文中已经提到，此处不再赘述。
2. **Image Captioning with RL**：与先前的一些尝试不同的是，本文将state作为RNN的输入而不是输出，因此本文可以构建一个独立的价值网络，而不是使用同一个RNN来构建actor与critic。
3. **Actor-Critic**：该模型通过critic生成决策的reward，并借此得到梯度来训练actor网络。AlpahGO即是该方法的典型应用之一。
4. **Sequence Generation**：当前有使用actor-critic结构解决机器翻译任务的尝试（同样是序列生成问题）。值得注意的是，在训练过程中常常需要引入多种技巧来**减小crtic网络结果的方差**，否则一些**罕见动作的reward往往会被大幅高估**，导致模型收敛的困难。

## 模型构造

### 问题公式化

我们将任务视为：对于给定的图像 $I$ ，生成Caption序列 $ Y = \lbrace y_1,\dots,y_T\rbrace, y_t \in D$，其中 $D$ 为词典。为了简化公式，我们认为Caption序列的长度始终为定值 $T$.

对于训练与测试过程中的每一步，我们有输入-输出对 $(I,Y)$ . 训练得到的序列生成模型对于给定的图像 $I$ 将预测Caption序列 $\hat{Y}$，并计算得分 $R(\hat{Y},Y)$ 来对模型进行优化与评价。此处的 $R$ 函数依据任务不同，可以有BLEU或CIDEr等多种选择。

在本任务中，我们采用经典的Encoder-Decoder结构（见下图）

![](Actor-Critic-Sequence-Training\4.png)

为了将Image Captioning任务转变成RL任务，我们考虑将Caption的生成过程视为**有限Markov决策过程**（MDP) $\lbrace S, A, P, R, \gamma \rbrace$. 在MDP中，状态 $S$ 由图像 $I$ 经过CNN的编码结果 $I_e$ 与当前生成的单词序列（决策序列）$\lbrace a_0, a_1, \dots, a_t \rbrace$ 两部分组成。因此，状态转移函数 $P$ 为：$s_{t+1} = \lbrace s_t, a_{t+1} \rbrace$ ，即 $a_{t+1}$ 成为新状态 $s_{t+1}$的一部分，并且同时获得reward $r_{t+1}$. 此外，$\gamma \in [0,1]$ 为discount factor. 因此，我们可以将该任务视为优化reward的标准RL任务。

### 模型组成部分

本模型采用Inception-V3作为CNN部分，LSTM作为RNN部分。策略网络（actor）与价值网络（critic）**都基于LSTM进行序列动作的决策和value的生成。**

#### 策略网络（actor）

基于对任务的描述，我们将 $I_e$ 和序列开始标记<start\> $a_0$ 进行组合得到起始状态 $s_0 = \lbrace I_e, a_0 \rbrace$. 因此，我们可以递归地得到**每个状态的表示方法**：
$$
s_t = \lbrace I_e, a_0, a_1, \dots, a_t \rbrace
$$

我们将状态 $s_t$ 送入LSTM中获得隐藏层 $h_{t+1}$. 为了获得词语生成的概率模型，我们增加一个**随机输出层** $f$ （常常用softmax作为激活函数），其结果为 $a_{t+1} \in D$.
$$
h_{t+1} = LSTM(s_t),
$$
$$
a_{t+1} \sim f(h_{t+1})
$$
因此，策略网络得出了在当前状态 $s_t$ 下，动作 $a_{t+1}$ 的概率分布 $p(a_{t+1}|s_t)$. 

#### 价值网络（critic）

在给定的策略 $\pi$，采样获得的动作与奖励函数下，value表示**期望的未来收益**（为 $s_t$ 的函数）。我们约定 $V$ 作为估计的state-value函数。

$$
V^\pi(s_t) = \mathbb{E}[\sum^{T-t-1}_{l=0}\gamma^lr_{t+l+1} \ |\  a_{t+1}, \dots, a_T \sim \pi, I]
$$

其中discount factor $\gamma$ 使得我们能够**以增加偏差为代价来减小方差**。它与MDP中出现的 $\gamma$ 是一致的。

本模型中的价值网络可以被视为一个**Encoder**。我们使用一个独立的LSTM层（参数记为 $\phi$ ），接受状态 $s_t$ 为输入，用其单一的输出值来预测 **TD 目标值**。

#### Advantage 函数

我们使用时序差分（TD）学习来获得Advantage. 我们定义Q值如下：
$$
Q^\pi(s_t,a_{t+1})=(1-\lambda)\sum^\infty_{n=1}G^n_t
$$
其中 $G^n_t$ 为n步的期望回报：
$$
G^n_t = r_{t+1} + \gamma r_{t+2} + \dots + \gamma^{n-1}r_{t+n} + \gamma^nV^\pi(s_t)
$$
因此，我们得到Adavantage函数的表示形式：
$$
A^\pi(s_t,a_{t+1})=Q^\pi(s_t,a_{t+1})-V^\pi(s_t)=(1-\lambda)\sum^\infty_{n=1}G^n_t-V^\pi(s_t)
$$

#### Value 函数

我们使用一个非线性函数来表示Value，最简单的方法就是解决一个非线性回归问题：
$$
argmin\ \phi \ ||Q^\pi(s_t,a_{t+1})-V^t_\phi(s_t)||^2
$$

其中 $Q^\pi(s_t,a_{t+1})$ 的定义已经在上文说明

#### λ 的选择

$\lambda$ 在整个算法中起到重要作用。如果 $\lambda=0$, 那么Advantage与Value的估计**只考虑单步TD**；如果 $\lambda=1$, 那么估计则转变成**蒙特卡洛方法**。因为Image Captioning任务中的序列长度较短，我们需要对整个序列进行采样来获得reward，所以我们令 $\lambda=1$, 这使得Advantage与Value的估计不会产生偏差，而有限的序列长度也限制了估计结果方差的大小。此时我们有：
$$
Q^\pi(s_t,a_{t+1}) = \sum^{T-t-1}_{l=0} \gamma^lr_{t+l+1}
$$

#### Reward

在本任务中，我们仅能在Caption序列完全生成时才能得到评价指标（如：CIDEr分数）。因此，我们定义reward如下：
$$
r_t =
\begin{cases}
0  & \text{if $t<T$} \\
score & \text{if $t=T$}
\end{cases}
$$
因此有：
$$
Q^\pi(s_t,a_{t+1})=\gamma^{T-t-1}r_T
$$

## 模型训练与评估

### 训练过程

1. CNN部分采用Inception-v3，RNN部分采用深度为512的LSTM层（与embedding维度相同）
3. 词典大小为12K（注意上述参数均与NICv2模型相同，便于对比）
4. actor-critic模型难以从头开始训练。
   原因：两者都**无法给对方提供有效的指示信号**。actor将会完全随机选择单词，并在critic网络中得到非常低的reward。
5. 训练步骤：
   1. （**预训练actor**）以**交叉熵**为标准，训练一个新的actor网络。
   2. （**预训练critic**）对经过预训练的actor网络（参数固定）的决策进行采样，用采样的结果训练critic网络。（2K次迭代，使用Adam优化器，学习率5e-5）
   3. （**联合训练**）使用Adam优化器，初始学习率5e-5，1M次迭代后降为5e-6，mini-batch大小为16，discount factor λ = 1（蒙特卡洛方法）
5. 词语选择方面，为了提高效率，采用简单的**贪婪算法**选择生成的词语。

### 训练结果

1. 本模型的表现相较于同时期的其他模型有较大提升，详情见下图。
	![](Actor-Critic-Sequence-Training\1.png)

2. 在计算成本方面，本模型是效果相似的模型中**效率最高**的一个。这主要是因为本模型**不包含attention单元**。另外，self-critical模型需要在每次迭代中**采样两次**（随机采样 + 贪婪算法解码），提高了算法的复杂度。详情见下图。
	![](Actor-Critic-Sequence-Training\2.png)

3. 以下为人工标注，优化交叉熵（XE）模型与本模型的结果示意图。可以看出本模型在结果的准确度上有较大的优越性。
	![](Actor-Critic-Sequence-Training\3.png)