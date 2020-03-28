# Tensorflow Cifar-10图像分类任务


基于TensorFlow的人脸识别智能小程序的设计与实现  Tensorflow Cifar-10图像分类任务

<!--more-->
## 1 Cifar10图像分类任务

* CIFAR-10数据集包含10小类，60000个`32*32`的彩色图像。有50000个训练图像和10000个测试图像
* CIFAR-100数据集包含100小类，每小类包含600个图像，其中有500个训练图像和100个测试图像。100类被分组为20个大类。每个图像带有1个小类的"fine”标签和1个大类"coarse"标签。

## 2 Cifar10图像数据解析

```Python
def unpickle(ile):
    import pickle
    with open(file,'rb') as fo:
        dict = pickle.load(fo,encoding='bytes')
    return dict
```

## 3 Cifar10数据打包和数据读取

```Python
tf.train.string input producer
Train.tfrecord
Test.tfrecord
```

## 4 TensorFlow训练框架搭建

* Data
* Net
* Loss
* Summary
* Session

## 5 TensorFlow挑战Cifar10编程案例

* 训练代码
* 测试代码
* Tensorboard调试
* 模型优化

## 6 如何优化Cifar10图像分类任务

* 更多的数据增强策略，比如: mixup等
* 更好的主干网络结构，比如: SENet等
* 更好的标签策略，比如: Soft-label策略
* 更好的loss设计，比如:采用分类+回归smooth-l1 loss等
* 不同的优化器、参数初始化方法等

## 7 实例代码

>
><https://github.com/ieblYang/CIFAR-10>
>
> -- _ieblYang_
