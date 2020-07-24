---
title: Pytorch
tags:
  - AI
  - pytorch
categories:
  - AI
abbrlink: AI/pytorch/pytorch_01
date: 2020-01-31 11:48:43
---

PyTorch中文手册第一章：PyTorch入门。包括简介，环境搭建，相关资源介绍和60分钟快速入门
<!--more-->

## 1.1 PyTorch简介

> Torch是一个与Numpy类似的张量（Tensor）操作库，与Numpy不同的是Torch对GPU支持的很好，Lua是Torch的上层包装。

Pytorch与Torch共享底层，Pytorch本身是一个python包，提供两个高级功能：
-  GPU加速的张量计算
-  自动求导的深度神经网络


## 1.2 Pytorch环境搭建

1. 安装 Pytorch
``` cmd
#默认 使用 cuda10.1
pip3 install torch===1.3.0 torchvision===0.4.1 -f https://download.pytorch.org/whl/torch_stable.

#cuda 9.2
pip3 install torch==1.3.0+cu92 torchvision==0.4.1+cu92 -f https://download.pytorch.org/whl/torch_stable.html

#cpu版本
pip3 install torch==1.3.0+cpu torchvision==0.4.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
```
2. 配置 Jupyter Notebook
```cmd
#安装ipykernel
conda install ipykernel
#写入环境
python -m ipykernel install  --name pytorch --display-name "Pytorch for Deeplearning"
#切换回基础环境
activate base
#创建jupyter notebook配置文件
jupyter notebook --generate-config
## 这里会显示创建jupyter_notebook_config.py的具体位置
```
​		打开文件，修改
```
c.NotebookApp.notebook_dir = '' 默认目录位置
c.NotebookApp.iopub_data_rate_limit = 100000000 这个改大一些否则有可能报错
```
3. 至此 Pytorch 的开发环境安装完成，可以在开始菜单中打开Jupyter Notebook 在New 菜单中创建文件时选择Pytorch for Deeplearning 创建PyTorch的相关开发环境了

****



## 1.3 60分钟快速入门

### 1.3.1 Tensor_tutorial

tensor的概念和用法参考numpy即可

- `torch.empty(m, n)`不是零张量，其初值是分配的存储空间的值

- 允许以相同的size重载tensor：`torch.randn_like(x, dtype=torch.float)`

- 一些定义tensor的方式

  ```python
  x = torch.empty(5, 3) #分配空间
  x = torch.rand(5, 3) #随机
  x = torch.zeros(5, 3, dtype=torch.long) #全零
  x = torch.tensor([[5,3], [5,3]]) #直接定义
  ```

- tensor的四则运算

  ```python
  # 假设x和y是相同大小的tensor，以add示范，其他的参考
  result = x + y
  result = torch.add(x, y)
  torch.add(x, y, out=result)
  y.add_(x) # in-place addition
  ```

- 所有in-place的运算都是正常的操作符加一个下划线 **_**
  如`x.copy_(y)` , `x.t_()`

- 支持切片操作（`x[:, 1]`）

- torch本身可以通过`x.view`来实现reshape

  ```python
  x = torch.rand(4, 4) # size: [4,4]
  y = x.view(16) # size: [16], 注意不是二维的[16,1]
  z = x.view(-1, 8) # size:[2,8], -1即由另一个维度决定，没有决定权的意思
  ```

- tensor和numpy可以相互转换，他们**共享同一个存储单元**

```python
# tensor to numpy
a = torch.ones(5)
b = a.numpy()
# numpy to tensor
a = np.ones(5)
b = torch.from_numpy(a)
```

****



### 1.3.2 Autograd_tutorial

		`autograd`本身是一个包，也是PyTorch中最重要的一个包，其中`torch.Tensor`就是这个包中最重要的一个类

- 内部操作
  每个变量有两个标志：`requires_grad` 和 `volatile`

  - **requires_grad**

    > 如果有一个单一的输入操作需要梯度，它的输出也需要梯度。相反，只有所有输入都不需要梯度，输出才不需要。如果其中所有的变量都不需要梯度进行，后向计算不会在子图中执行。

    		即对于某一`requires_grad`为`True`的tensor输出方向的tensor都要为`True`，输入方向的tensor不做要求。
																																													
    		因此在冻结模型某部分的时候不需要改变网络结构，只需要切换`requires_grad`的地方就可以了

  - **volatile**
    利用这个标志的前提是所有tensor的`requires_grad`都为`False`，优势是梯度更容易传递，会使用绝对最小的内存来评估模型。感觉用不上所以就没有细看了

- 如果不想自动计算所有梯度（应该用不上）： `with torch.no_grad():`, 把想要计算梯度的tensor的`requires_grad`标志置为`True`即可。在评估模型时很有用

****



### 1.3.3 Neural_Networks_tutorial

		利用`torch.nn`包来搭建神经网络

- **Define the network**

  一般定义网络即定义一个类（先不考虑后面的实例化），其中必须定义的两个成员函数是`__init__(self)` 和 `forward(self, x)`。其中__init__函数用来定义每层的结构，forward函数用来按顺序排列每层的结构。
  ```python
  import torch
  import torch.nn as nn
  import torch.nn.functional as F

  class Net(nn.Module):
  def __init__(self):
      super(Net, self).__init__()
      # 1 input image channel, 6 output channels, 5x5 square convolution
      # kernel
      self.conv1 = nn.Conv2d(1, 6, 5)
      self.conv2 = nn.Conv2d(6, 16, 5)
      # an affine operation: y = Wx + b
      self.fc1 = nn.Linear(16 * 5 * 5, 120)
      self.fc2 = nn.Linear(120, 84)
      self.fc3 = nn.Linear(84, 10)
      
    def forward(self, x):
      # Max pooling over a (2, 2) window
      x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
      # If the size is a square you can only specify a single number
      x = F.max_pool2d(F.relu(self.conv2(x)), 2)
      x = x.view(-1, self.num_flat_features(x))
      x = F.relu(self.fc1(x))
      x = F.relu(self.fc2(x))
      x = self.fc3(x)
      return x
  
    def num_flat_features(self, x):
      size = x.size()[1:]  # all dimensions except the batch dimension
      num_features = 1
      for s in size:
          num_features *= s
      return num_features
  
  net = Net()	# 实例化
  ```



- **Processing inputs and calling backward**

  ​		对于LeNet来说，图像大小是32×32的，**不要把conv卷积核大小和输入大小混淆**，所以定义输入和反向传播计算如下：

  ```python
  # 因为第一层是conv，所以要按照nn.Conv2d的输入参数来定
  # nn.Conv2d接受一个4维的张量，sSamples * nChannels * Height * Width（样本数*通道数*高*宽）
  input = torch.randn(1, 1, 32, 32) # torch.nn不支持单个样本，所以必须多一个维度来表示个数
  out = net(input)
  
  # 反向传播
  net.zero_grad() # 将所有参数的梯度缓存清零
out.backward(torch.randn(1, 10)) # 随机梯度的反向传播
  # （1是输入决定的样本个数，10是网络定义的输出个数）
  ```
  
  

- **Compute the loss**

  ​		即构造一个损失函数，以一对 `(output, target)` 作为输入，计算一个scalar估计网络输出和目标值的差值。 nn包中有很多不同的损失函数，比如 `nn.MSELoss`。

  ​		用法如下：

  ```python
  output = net(input)
  target = torch.randn(10)  # 随机值作为样例
  target = target.view(1, -1)  # 使target和output的shape相同
  criterion = nn.MSELoss()
  
  loss = criterion(output, target)
  print(loss)
  ```




- **Backprop**

  ​		简单来说，一句 `loss.backward()` 即可。以下展示调用 `loss.backward()` 的效果：

  ```python
  net.zero_grad()     # 一定记得清除梯度
  # 这里仅展示conv1层的bias项在反响传播前后的梯度，因为有6个output channel，所以有6项
  print('conv1.bias.grad before backward') 
  print(net.conv1.bias.grad)
  
  loss.backward()
  
  print('conv1.bias.grad after backward')
  print(net.conv1.bias.grad)
  ```



- **Update the weights**

  ​			Pytorch构建了一个包 `torch.optim` 实现了各种更新规则，使用方法如下：

  ```python
import torch.optim as optim
  
  # create your optimizer
  optimizer = optim.SGD(net.parameters(), lr=0.01)
  
  # in your training loop:
  optimizer.zero_grad()   # zero the gradient buffers （注意也要将梯度缓冲区置零
  output = net(input)
  loss = criterion(output, target)
  loss.backward()
  optimizer.step()    # Does the update
  ```

****

### 1.3.4 Cifar10_tutorial

> 依次按照下列顺序进行：
> 1. 使用torchvision加载和归一化CIFAR10训练集和测试集
> 2. 定义一个卷积神经网络（CNN）
> 3. 定义损失函数（Loss func）
> 4. 在训练集上训练网络
> 5. 在测试集上测试网络

- **处理数据集**

  > CIFAR10数据集有如下10个类别：‘airplane’, ‘automobile’, ‘bird’, ‘cat’, ‘deer’, ‘dog’, ‘frog’, ‘horse’, ‘ship’, ‘truck’。CIFAR-10的图像都是 3x32x32大小的，即，3颜色通道，32x32像素。

  ​		`torchvision` 包可以直接处理CIFAR10等基本图像数据集，可以用图像转换器`torchvision.datasets` 和 `torch.utils.data.Dataloader`来分别得到 test 和 train 数据。

  ```python
  import torch
  import torchvision
  import torchvision.transforms as transforms
  
  # torchvision的输出是[0,1]的PILImage图像，我们把它转换为归一化范围为[-1, 1]的张量
  transform = transforms.Compose(
      [transforms.ToTensor(),
       transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])
  # 下载训练集并读取
  trainset = torchvision.datasets.CIFAR10(root='./data', train=True,
                                          download=True, transform=transform)
  trainloader = torch.utils.data.DataLoader(trainset, batch_size=4,
                                            shuffle=True, num_workers=2)
  # 下载测试集并读取
  testset = torchvision.datasets.CIFAR10(root='./data', train=False,
                                         download=True, transform=transform)
  testloader = torch.utils.data.DataLoader(testset, batch_size=4,
                                           shuffle=False, num_workers=2)
  # 10个tuple贴上标签
  classes = ('plane', 'car', 'bird', 'cat',
             'deer', 'dog', 'frog', 'horse', 'ship', 'truck')
  ```

  ​		通过以下方法**展示图像**：

  > plt.imshow()函数负责对图像进行处理，并显示其格式，而plt.show()则是将plt.imshow()处理后的函数显示出来。
  >
  > 关于np.transpose: https://blog.csdn.net/u012762410/article/details/78912667

  ```python
  import matplotlib.pyplot as plt
  import numpy as np
  
  # 展示图像的函数
  def imshow(img):
      img = img / 2 + 0.5     # unnormalize, 从[-1, 1]变回[0, 1]
      npimg = img.numpy()		# 转换成numpy格式
      plt.imshow(np.transpose(npimg, (1, 2, 0)))	
      plt.show()
  
  # **获取随机数据**
  dataiter = iter(trainloader)	# iter的对象是Dataloader
  images, labels = dataiter.next()
  # 展示图像
  imshow(torchvision.utils.make_grid(images))	# make_grid的作用是把四张图片拼在一起
  # 显示图像标签
  print(' '.join('%5s' % classes[labels[j]] for j in range(4)))
  ```

  

- **定义CNN**

  参考上一节（只是把单通道变为三通道）：

  		- relu等激活函数在 `torch.nn.functional` 包中
  		- conv层，pooling层，fc层等layer在 `torch.nn` 包中

  ```python
  import torch.nn as nn
  import torch.nn.functional as F
  
  class Net(nn.Module):
      def __init__(self):
          super(Net, self).__init__()
          self.conv1 = nn.Conv2d(3, 6, 5)
          self.pool = nn.MaxPool2d(2, 2)
          self.conv2 = nn.Conv2d(6, 16, 5)
          self.fc1 = nn.Linear(16 * 5 * 5, 120)
          self.fc2 = nn.Linear(120, 84)
          self.fc3 = nn.Linear(84, 10)
  
      def forward(self, x):
          x = self.pool(F.relu(self.conv1(x)))
          x = self.pool(F.relu(self.conv2(x)))
          x = x.view(-1, 16 * 5 * 5)
          x = F.relu(self.fc1(x))
          x = F.relu(self.fc2(x))
          x = self.fc3(x)
          return x
  
  net = Net()
  ```
  
- **定义Loss Function 和 Optimizer**

  ​		交叉熵作为损失函数，带动量的SGD作为优化器（RMS Prop）

  ```python
  import torch.optim as optim
  
  criterion = nn.CrossEntropyLoss()
  optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)
  ```

  

- **Training**

  几个要点：

  -  两个循环，先放外面的大循环表示epoch，循环次数为超参; 里面的小循环表示遍历训练集，标准语句为`for i, data in enumerate(trainloader, 0):`
  - running_loss 每次清零要在两个循环之间，即只是统计每个 epoch 的 loss
  - 小循环中：获取输入，梯度清零，正向传播，计算损失，反向传播，优化，更新loss状态并打印

  ```python
  for epoch in range(2):  # 多批次循环
  
      running_loss = 0.0
      for i, data in enumerate(trainloader, 0):
          # 获取输入
          inputs, labels = data
  
          # 梯度置0
          optimizer.zero_grad()
  
          # 正向传播，反向传播，优化
          outputs = net(inputs)	# 正向传播
          loss = criterion(outputs, labels)	# 计算损失
          loss.backward()		# 反向传播
          optimizer.step()	# 优化
  
          # 打印状态信息
          running_loss += loss.item()
          if i % 2000 == 1999:    # 每2000批次打印一次
              print('[%d, %5d] loss: %.3f' %
                    (epoch + 1, i + 1, running_loss / 2000))
              running_loss = 0.0
  
  print('Finished Training')
  ```

  

- **Test**

  ​		通常采用全体测试集进行检验，计算正确率，这里先具象展示训练效果：

  ```python
  dataiter = iter(testloader)
  images, labels = dataiter.next()
  
  # 显示图片和GroundTruth
  imshow(torchvision.utils.make_grid(images))
  print('GroundTruth: ', ' '.join('%5s' % classes[labels[j]] for j in range(4)))
  
  # 显示预测结果
  outputs = net(images)
  _, predicted = torch.max(outputs, 1)	# 最高概率的标签
  print('Predicted: ', ' '.join('%5s' % classes[predicted[j]] for j in range(4)))
  ```

  ​		计算整体正确率的代码这样来写：

  ```python
  correct = 0
  total = 0
  with torch.no_grad():
      for data in testloader:
          images, labels = data
          outputs = net(images)
          _, predicted = torch.max(outputs.data, 1)
          total += labels.size(0)
          correct += (predicted == labels).sum().item()
  
  print('Accuracy of the network on the 10000 test images: %d %%' % (100 * correct / total))
  ```

  ​		具体看某一类的正确率：

  ```python
  class_correct = list(0. for i in range(10))
  class_total = list(0. for i in range(10))
  with torch.no_grad():
      for data in testloader:
          images, labels = data
          outputs = net(images)
          _, predicted = torch.max(outputs, 1)
          c = (predicted == labels).squeeze()	#二维变一维
          for i in range(4):
              label = labels[i]
              class_correct[label] += c[i].item()
              class_total[label] += 1
  
  for i in range(10):
      print('Accuracy of %5s : %2d %%' % (classes[i], 100 * class_correct[i] / class_total[i]))
  ```

  

  
  
  
## 1.4 Data Loading

### Dataset

​		Dataset是一个抽象类，为了能够方便的读取，需要将要使用的数据包装为Dataset类。 自定义的Dataset需要继承它并且实现两个成员方法：

1. `__getitem__()` 该方法定义用索引(`0` 到 `len(self)`)获取一条数据或一个样本
2. `__len__()` 该方法返回数据集的总长度

   ​		以下举一个姿态估计的例子，label通过` __init__()`函数获取，训练图片通过`__getitem__()`函数获取，这样比较节约内存，一般是通过字典存储，格式如`{'image': image, 'landmarks': landmarks}`。Dataset 的定义和调用方法如下：

  ```python
  from torch.utils.data import Dataset
  import pandas as pd
  
    # 定义数据集
  class MyData(Dataset):
  	
    def __init__(self, csv_file, root_dir, transform=None):
        self.landmarks_frame = pd.read_csv(csv_file)
        self.root_dir = root_dir
        self.transform = transform
        
    def __len__(self):
    	return len(self.landmarks_frame)
    
    def __getitem__(self, idx):
        if torch.is_tensor(idx):
            idx = idx.tolist()
            
        img_name = os.path.join(self.root_dir, 
                                self.landmarks_frame.iloc[idx, 0])
        image = io.imread(img_name)
        landmarks = self.landmarks_frame.iloc[idx, 1:]
        landmarks = np.array([landmarks])
        sample = {'image': image, 'landmarks': landmarks}
        
        if self.transform:
            sample = self.transform(sample)
            
        return sample
    
    
     # 调用数据集
    face_dataset = MyData(csv_file='data/faces/face_landmark.csv', 
                          root_dir='data/faces')
    
    for i in range(len(face_dataset)):
        sample = face_dataset[i]
        # 至此即可通过调用sample['image']或者sample['landmarks']来获取某一项的img和label
        
        
  ```

  ### Transforms

实际场景中，经常需要对图像进行预处理，其中必要的就是统一图像大小 (Rescale)，常见的有RandomCrop进行数据增强，一般来说需要建立一个类，成员函数由`__init__()` 和 `__call__()` 组成：

  ```python
class Rescale(object):
    
  ```

### DataLoader

DataLoader为我们提供了对Dataset的读取操作，常用参数有：batch_size(每个batch的大小)、 shuffle(是否进行shuffle操作)、 num_workers(加载数据的时候使用几个子进程)。下面做一些简单的操作：
  ```python
dataloader = DataLoader(face_dataset, batch_size=4,
                        shuffle=True, num_workers=0)
# 一般通过for循环处理每一个batch
for i_batch, sample_batched in enumerate(dataloader):
  ```