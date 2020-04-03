---
title: 比较 对抗样本 在 压缩模型 上的可迁移性
date: 2019-12-06 10:50:43
tags: [网络压缩,对抗样本,水文章]
categories: 
- AI
---

### 论文名称
[To Compress or Not to Compress: Understanding the Interactions between Adversarial Attacks and Neural Network Compression](https://arxiv.org/pdf/1810.00208.pdf)

### 论文重述
该论文主要是讨论对抗攻击在压缩模型上的可迁移性 (Transferability)，分为三种情形：
- compressed 产生对抗样本测试自身
- baseline 产生对抗样本测试 compressed
- compressed 产生对抗样本测试 baseline
比较的压缩方式为：剪枝 和 量化
比较的攻击方法为：IFGSM, IFGM, Deepfool

### 比较结果
- 剪枝方法
  1. 对于 IFGM 和 IFGSM 方法，对抗样本的迁移性是受到剪枝过后密度减小造成影响的。具体表现在：
     - 当密度小的时候，用compressed生成的对抗样本去攻击baseline的效果不错，从小到大迁移性好
     - 当密度小的时候，用baseline生成的对抗样本去攻击compressed效果不好，从大到小迁移性不好

  2. 对于 Deepfool 方法，小的生成样本攻击自身，小的生成样本攻击大的，这两种情况在密度减小的时候迁移性都更好。

  3. 因为clipped的原因，对于有clipped的方法（IFGSM and IFGM），小的生成样本攻击自身，大的生成样本攻击小的，这两种情况在密度减小的时候迁移性都更不好。

- 量化方法
  1. 当量化精度高的时候，在量化与不量化之间的攻击效果不受影响

  2. 当小数位数减小的很厉害时类似fine-grained pruning（剪枝权重）的效果

  3. 整数位数很少的时候，小的产生样本攻击大的会变得异常困难，表明类别的语义信息保存在activations and weights中

### 总结
***（以下仅表示个人观点，欢迎讨论指正）***

就像看了一份实验报告一样，没有新方法，主要是比较结果。

个人感觉方法分的太细了，特别是攻击方法，感觉 ①没有从攻击原理的角度阐释可迁移性，②只是众多攻击模型中的典型三种，且IFGSM，IFGM和Deepfool的区别还挺大的，感觉又只像两类攻击方法。总之感觉脱离数学原理的比较都显得不那么全面。

亮点：
- use Mayo tool to generate pruned and quantised models
- 对于应用的讨论，作者最终的结论是公司用压缩模型发行并不会增加大模型的安全性