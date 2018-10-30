---
title: pytorch tutorial
urlname: pytorch-tutorial
categories: dl
tags: [dl, pytorch]
date: 2018-10-24 11:40:58
mathjax: false


---

tutorial 地址: [pytorch: Training a Classifier](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html).

当使用新的数据集进行测试时, 出现的问题及解决的方法.

## Problem 1

error:

``` python
RuntimeError: invalid argument 0: Sizes of tensors must match except in dimension 0. Got 484 and 549 in dimension 2 at /pytorch/aten/src/TH/generic/THTensorMath.cpp:3616
```

location:

``` python
images, labels = data_iter.next()
```

solution:

``` python
数据集中的图像大小不一致.
需要使用`transforms.Resize([height, width])`把所有图像缩放到同一大小.
```

## Problem 2

error:

``` python
RuntimeError: invalid argument 2: size '[-1 x 400]' is invalid for input with 719104 elements at /pytorch/aten/src/TH/THStorage.cpp:80
```

location:

``` python
x = x.view(-1, 16 * 5 * 5)
```

solution:

``` python
`Tensor.view()` 相当于 `numpy.reshape()` 方法, 即重塑形状.
其中`-1`表示依据其他维度进行推理得出的维度.
这里的参数需要计算得出, 不同的输入尺寸需要计算对应的参数!
---
我们来计算一下, 计算公式见下面的图片.
---
input size = 3*32*32
-
class Net_t1(nn.Module):
    def __init__(self):
        super(Net_t1, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = F.relu(self.conv1(x))
        x = self.pool(x)
        x = F.relu(self.conv2(x))
        x = self.pool(x)
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
-
1 torch.Size([4, 6, 28, 28]), conv1, (32 - 5) / 1 + 1 = 28, padding=0
2 torch.Size([4, 6, 14, 14]), pool, 28 / 2 = 14
3 torch.Size([4, 16, 10, 10]), conv2, (14 - 5) / 1 + 1 = 10, padding=0
4 torch.Size([4, 16, 5, 5]), pool, 10 / 2 = 5
5 torch.Size([4, 400]), view, 16 * 5 * 5 = 400
6 torch.Size([4, 120]), full_connect
7 torch.Size([4, 84]), full_connect
8 torch.Size([4, 10]), full_connect
---
input size = 3*224*224
-
class Net_t2(nn.Module):
    def __init__(self):
        super(Net_t2, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 53 * 53, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 31)

    def forward(self, x):
        x = F.relu(self.conv1(x))
        x = self.pool(x)
        x = F.relu(self.conv2(x))
        x = self.pool(x)
        x = x.view(-1, 16 * 53 * 53)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
-
1 torch.Size([4, 6, 220, 220]), conv1, (224 - 5) / 1 + 1 = 220, padding=0
2 torch.Size([4, 6, 110, 110]), pool, 220 / 2 = 110
3 torch.Size([4, 16, 106, 106]), conv2, (110 - 5) / 1 + 1 = 106, padding=0
4 torch.Size([4, 16, 53, 53]), pool, 106 / 2 = 53
5 torch.Size([4, 44944]), view, 16 * 53 * 53 = 44944
6 torch.Size([4, 120]), full_connect
7 torch.Size([4, 84]), full_connect
8 torch.Size([4, 31]), full_connect
```

![1540383351683](assets/pytorch_tutorial/1540383351683.png)

## Problem 3

error:

``` python
RuntimeError: Assertion `cur_target >= 0 && cur_target < n_classes' failed.  at /pytorch/aten/src/THNN/generic/ClassNLLCriterion.c:93
```

location:

``` python
loss = criterion(outputs, labels)
```

solution:

``` python
预测的标签向量和实际的标签向量维度不一致!
设置输出层(最后一层)神经元个数为真实的标签个数.
```

## Problem 4

error:

``` python
RuntimeError: Expected object of type torch.FloatTensor but found type torch.cuda.FloatTensor for argument #2 'weight'
```

location:

``` python
net = Net().to(device)
```

solution:

``` python
在做计算时, 需要把所有需要计算的量都放在`device`上面. 因此不仅网络需要放在`device上面`, `inputs`和`labels`也要放在`device`上面.
```

## 其他

1. `torchvision.datasets.ImageFolder()`会自动加载标签信息.
   - 可以通过上述语句返回的对象调用`len(dataset)`返回样本个数, 调用`dataset.classes`返回标签集合.

