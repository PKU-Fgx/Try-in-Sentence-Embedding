### 一、思路

1. Sentence 1: `[CLS_1] A B C D E F [SEP_1]`
2. Cpt_sentence: `[CLS_2] C D [SEP_2]`
3. Sentence 2: `[CLS_3] A B [CLS_2] E F [SEP_3]`
4. 理论上讲，`[CLS_3]` 与 `[CLS_1]` 是相互接近的，用对比损失来进行训练；

### 二、情况

1. 同样的 Setting 下，SimCSE 我粗糙复现的无监督版本的效果要比我的思路平均高上 4 个点；
2. SentEval 的 SICK-R 分数统一偏高，不知道怎么回事；

### 三、阻碍

#### I. 输出空间和输入空间分布不一致的问题

1. 表征转换？将输出空间分布转换为输入空间分布；

#### II. 网络 Loss 很容易降的很低，为什么？

1. 增大数据试一试；

2. Hard Sample 没有去做，只是 Batch 内做负样本不太行，具体参考 SimCSE 论文；

### 四、设想

#### I. 在SimCSE上继续预训练（在做）

1. 在自己简单的实现SimCSE的基础上，发现 Loss 会从一个较低的值（0.07 左右）开始降低，说明我的方法和 SimCSE 的表征相近，但是存在偏差；

2. 在 Hugging Face 上下载下来的 `princeton-nlp/unsup-simcse-bert-base-uncased` 基础上进行预训练，Loss 从一个不高的值（0.15 左右）开始降低，偏差更大了；

#### II. 把一个 span 的 token 的表征 Avg 一下输入作正样本

### 五、实验
