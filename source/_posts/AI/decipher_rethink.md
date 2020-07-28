---
title: 代码解析 for <Rethinking the value of Network Pruning>
date: 2020-07-27 10:50:43
tags: [网络压缩, 剪枝]
categories: 
- AI
---

不得不说有这么个资源整合的论文,配套的开源代码还是非常友好的,在CIFAR-10数据集上分别使用了layerwise的l1-norm通道剪枝和ThiNet,global的network slim,unstructured的song han的开山之作,还有个soft prune,涉及面非常广,代码易读性强.而且因为代码风格一样,所以看懂一个就可以非常快速的掌握剩下三个.
<!--more-->

原论文:[Rethinking the Value of Network Pruning](https://arxiv.org/abs/1810.05270)

这里只解析resnet56的网络(最实用并且比较简单的网络),其他结构大同小异.

### l1-norm-pruning
原论文:[L1-norm based channel pruning](https://arxiv.org/abs/1608.08710)

1. **代码结构:**
|- models
|  |- __init__.py
|  |- resnet.py
|  |- vgg.py
|- compute_flops.py
|- main_B.py
|- main_E.py
|- main_finetune.py     # finetune
|- main.py              # 训练
|- res56prune.py        # 剪枝在此
|- res110prune.py
|- vggprune.py

2. **网络结构:**对于resnet56来说, 3layers,每个layers有9个block => 18 layer
(conv1): Conv2d(3, 16, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
  (bn1): BatchNorm2d(16, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (relu): ReLU(inplace=True)
  (layer1): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(16, 16, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(16, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(16, 16, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(16, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
    相同Block结构一直到(8),总共9个block,此处省略8个
  )
  (layer2): Sequential(
    layer1的所有16变32,所以此处省略18层
  )
  (layer3): Sequential(
    layer1的所有16变64,所以此处省略18层
  )
  (avgpool): AvgPool2d(kernel_size=8, stride=8, padding=0)
  (fc): Linear(in_features=64, out_features=10, bias=True)
  
3. **快速上手**
    `compute_flops.py` 是给 `main_B.py` 准备的,`main_B.py`和`main_E.py`可以一样看待,他们作用都是最后类似fine-tune的工作,所以和`main_finetune.py`差不多,但是比他多了调整lr的操作,同时B比E多考虑了flops变化作为终止条件.然后`main.py`就是正常训练.`models/resnet.py`和`res56prune.py`这两个文件就是核心所在了.
  - `resnet.py`:网络结构重述:1+18+18+18+1.里面有个cfg的参数,因为剪的是通道,即网络结构有在改变,而cfg就是固定网络结构的参数.
  - `res56prune.py`:
    - 注意哪些要剪的:1. 偶数层(刚好就是无跳连的层=>有跳连的层不剪,第一层conv不剪)；2. 原文提到了一些比较脆弱的部分(与featuremap改变有关的层)不剪,即部分偶数层不剪 .
    - 原文有两种剪枝模式:A/B,差别在于剪枝力度(same/weighted)和剪枝对象的略微不同(B不剪的对象会更多一点).
    - 过两遍model,第一遍根据weight.data确定cfg_mask,第二遍根据cfg_mask生成新网络.这也解决了我一直以来的困惑,究竟怎么剪掉一整个通道,原来是生成一个新网络啊.注意cfg_mask标识的是每层的具体哪些filter要剪,cfg标识的是每层剪了剩下多少个filter,然后在保存模型时只保存了cfg,没有保存cfg_nask.
    - new_model继承的时候注意其他参数也要一并跟上(BN,不剪的conv等)

### weight-level
原论文:[Non-structured weight-level pruning](https://arxiv.org/abs/1506.02626)

1. **代码结构**:其实和上一个一模一样
|- models
|  |- __init__.py
|  |- cifar
|  |  |- __init__.py
|  |  |- resnet.py
|  |  |- vgg.py
|  |  |- preresnet.py
|  |  |- densenet.py
|  |  |- alexnet.py
|- utils                # acc, logger, getters, visualize一些真正辅助的东西
|- cifar_B.py
|- cifar_E.py
|- cifar_finetune.py    # 正常finetune
|- cifar.py             # 正常train
|- cifar_prune.py       # 正常prune
|- count_flops.py
|- vggprune.py

2. **网络结构**
   因为都是resnet56,所以与上一个网络模型一样

3. **快速上手**
   `models/cifar/resnet.py`会比上一个的更完整,上一个只有BasicBlock,这一部分的resnet还有BottleNeck,其他一模一样.重点说说`cifar_prune.py`:先获取所有conv2d的weights并排序,通过先验的args.percent确定thre.之后开始剪,剪的部分也非常简单粗暴,根据是否大于thre生成一个mask,然后直接`m.weight.data.mul_(mask)`就好
