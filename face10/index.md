# 人脸对齐/关键点训练业务实战


基于TensorFlow的人脸识别智能小程序的设计与实现 人脸对齐/关键点训练业务实战

<!--more-->

## 1 人脸对齐/关键点业务介绍


* 根据输入的人脸图像，自动定位出面部特征关键点，如眼睛、鼻尖、嘴角点、眉毛以及人脸各部件轮廓点等  
![Minion](/images/face/face10/1.jpg)

* 2D人脸
* 3D人脸
* 关键点数量：5,21,29,68,96,192......
* 旷视科技`1000`点与`8000`点对比如下图  

![Minion](/images/face/face10/2.jpg)

* 表情识别、人脸编辑、人脸美妆、三维重建

## 2 人脸关键点算法评价指标

### 2.1 通常将输出结果表示成点的集合，再进一步表示成向量

### 2.2 NME：Normalized mean error

* 两外眼角间距离
* 人脸外接矩阵对角线长度
$$ e = \frac{\textstyle\sum_{i=1}^n \Vert X_{(i)}^e-X_{(i)}^e \Vert_2}{N * d_{io}}$$

### 2.3 CED：Cumulative Errors Distribution（CED）curve

![Minion](/images/face/face10/3.jpg)

* 横坐标表达当前的偏差值
* 纵坐标表达满足当前偏差值得图片的数量

## 3 人脸关键点方法介绍

### 3.1 传统方法

#### 3.1.1 形状学习

* 基于形状学习的模型：ASM、AAM
* 基于形状学习的人脸关键点流程 

![Minion](/images/face/face10/4.jpg)

<<<<<<< HEAD
{{< admonition note "笔记">}}
=======
{{% admonition note "笔记"%}}
>>>>>>> add / update posts
* 主要作用：勾勒出人脸基准点的轮廓，轮廓属于特定的形状。
* 人脸模型：对人脸特征点进行建模，首先选择基准图片，利用基准图片作为参照，
将训练集的图片按照基准进行变换，得到训练集的图像处理后的集合，
对训练集中人脸关键点标定的位置，对每一个点进行特征提取，以当前关键点为中心，
提取它的局部的图像区域进行特征提取，假设特征提取后有5个点，就会得到5个向量。
* 点搜索：给定一个测试集的图片，进行关键点的搜索，
初步搜索时选择5个点中的某几个点作为最开始搜索的对象，通常选择眼睛或鼻子。
* 对齐：根据基准点对人脸进行对齐。
* 其他点的搜索定位：以基准点为参照，预测想要定位的其他的点的大致位置！
<<<<<<< HEAD
{{< /admonition >}}
=======
{{% /admonition %}}
>>>>>>> add / update posts

#### 3.1.2 级联回归学习

* 基于级联回归学习的模型：CPR  

![Minion](/images/face/face10/5.jpg)

<<<<<<< HEAD
{{< admonition note "笔记" >}}
=======
{{% admonition note "笔记" %}}
>>>>>>> add / update posts
* 对于人脸特征点定位，人脸关键点检测的目的是估计向量(Facial Shape) $S=(x_1,y_1,x_2,y_2,...,x_K,y_K)$ ，
其中 $K$ 表示关键点的个数，由于每个关键点有横纵两个坐标，所以 $S$ 的长度为 $2K$ 。 
对于一个输入 $I$ , 给定一个初始形状 $S^0$ (通常是在训练集计算得到的平均形状)。
每一级输出的是根据输入图像得到的偏移估计$\Delta S$，那么每一级都会更准确的预测
脸上Landmark的位置
$$S^{t+1 }= S^t+r_t(\phi(I,S^t))$$
其中，$S^t$ 和 $S^{t+1}$ 分别表示第 $t$ 和 $t+1$ 级预测的人脸形状(即所有关键点集合)，表示回归函数
* CPR通过一系列回归器将一个指定的初始预测值逐步细化，
每一个回归器都依靠前一个回归器的输出来执行简单的图像操作，
整个系统可自动的从训练样本中学习。
* CPR检测流程一共有T个阶段，在每个阶段中首先进行特征提取f，
这里使用的是shape-indexed features，
也可以使用诸如HOG、SIFT等人工设计的特征，
或者其他可学习特征（learning based features），
然后通过训练得到的回归器R来估计增量ΔS( update vector)，
把ΔS加到前一个阶段的S上得到新的S，这样通过不断的迭代即可以得到最终的S(shape)。
<<<<<<< HEAD
{{< /admonition >}}
=======
{{% /admonition %}}
>>>>>>> add / update posts

> 推荐阅读文献  
> * Face Alignment at 3000 FPS via Regressing Local Binary Features(LBF)
> * Joint Cascade Face Detection and Alignment
> * One Millisecond Face Alignment with an Ensemble of Regresion Trees(ETR)
> * Face Alignment In-the-Wild: A Survey
> * Facial feature point detection: A comprehensive survey

### 3.2 深度学习方法  

![Minion](/images/face/face10/6.jpg)

#### 3.2.1 多级回归

* DCNN
	* 总体思想是由粗到细，实现5个人脸关键点的精确定位。网络结构分为3层：level 1、level 2、level 3。每层都包含多个独立的CNN模型，负责预测部分或全部关键点位置，在此基础上平均来得到该层最终的预测结果。  
  
![Minion](/images/face/face10/7.jpg)

* DCNN-Face++

#### 3.2.2 多任务

* TCDCN
	* 使用与人脸相关的属性共同来学习人脸的特征点位置，通过这种多任务的学习，来提高人脸特征点检测的鲁棒性。具体而言，就是在人脸特征点检测时候，同时进行多个任务（包括性别、是否戴眼镜、是否微笑以及脸部姿势）的学习。使用这些辅助属性可以帮助更好的定位特征点。

* MTCNN
	* CNN回归和检测多任务，多尺度级联，三个网络级联，由粗到精，同时完成检测和特征点定位回归。 

#### 3.2.3 直接回归

* Vanilla CNN
	* 作者对网络不同层的特征进行使用GMM进行聚类分析，发现网络进行的是层次的，由粗到精(hierarchical, coarse to fine)的特征定位，越深的网络特征越能反应出特征点的位置。

#### 3.2.4 热图

* DAN
	* 与以往级联神经网络输入的是图像的某一部分不同，DAN各阶段网络的输入均为整张图片。当网络均采用整张图片作为输入时，DAN可以有效的克服头部姿态以及初始化带来的问题，从而得到更好的检测效果。之所以DAN能将整张图片作为输入，是因为其加入了关键点热图（Landmark Heatmaps），关键点热图的使用是本文的重要创新点。

#### 3.2.5 3D人脸关键点定位

* 2D人脸主要为可见点信息，对于侧脸很难训练
* 正脸到侧脸姿态变化较大，且标注十分困难

* 人脸本身就具有深度信息
	* Dense Face Alignment
	* DenseReg
	* FAN
	* 3DDFA
	* PRNet

## 4 人脸关键点常用数据集

| 数据集 | 个数 |
| ------ | ---- |
|[BiolD](https://bioid.com/About/BioID-Face-Database) |20|
|[LFPW](http://neerajkumar.org/databases/lfpw/)   |29|
|[AFLW](https://lrs.icg.tugraz.at/research/aflw/)|21|
|[COFW](http://www.vision.caltech.edu/xpburgos/)|29|
|[ICCV13/MVFW](https://sites.google.com/site/junliangxing/codes)|68|
|[OCFW](https://sites.google.com/site/junliangxing/codes)|68|
|[300-W](http://ibug.doc.ic.ac.uk/resources/300-W_IMAVIS/)|68|
|[HELEN](http://www.f-zhou.com/fa_code.html)|29|
|[CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)|5|  
  
* 300W-LP
	* 68点
	* AFW,LFPW,HELEN,IBUG and XM2VTS  

![Minion](/images/face/face10/8.jpg)

* Dlib库  
	* 能够检测出当前图片中的人脸的位置，并且完成人脸定位（68点），生成深度学习输入的训练样本。  

![Minion](/images/face/face10/9.jpg)

## 5 人脸关键点定位问题挑战及解决思路

|  问题  | 解决思路 |
| ------ | ---- |
|环境的变化|数据增强|
|姿态的变化|姿态分类、人脸对齐（矫正）|
|表情的变化|数据增强，GAN|
|遮挡问题|3D人脸关键点定位、优化主干网络|
|稠密点|3D人脸关键点定位、优化主干网络|
  
## 6 编程实战及模型优化

TensorFlow+SENet-人脸关键点定位编程实战及模型优化

### 6.1 TensorFlow+SENet模型详细介绍

* 人脸关键点网络结构
  
![Minion](/images/face/face10/10.jpg)

{{% admonition note "笔记" %}}
* 数据：训练样本
* 网络结构：SENet
* 输出：预测人脸中的68个关键点，表示为`68*2=136`维的向量
* 回归网络
{{% /admonition %}}


* 网络结构
  
![Minion](/images/face/face10/11.jpg)

* SENet模型详细介绍  
  
![Minion](/images/face/face10/12.jpg)

{{% admonition note "笔记" %}}

* SE-Inception Module 是将SE模块嵌入到Inception结构的一个示例。方框旁边的维度信息代表该层的输出。
这里我们使用global average pooling作为Squeeze操作。紧接着两个Fully Connected 
层组成一个Bottleneck结构去建模通道间的相关性，并输出和输入特征同样数目的权重。
我们首先将特征维度降低到输入的1/16，然后经过ReLu激活后再通过一个Fully Connected 
层升回到原来的维度。这样做比直接用一个Fully Connected层的好处在于：
	* 具有更多的非线性，可以更好地拟合通道间复杂的相关性；
	* 极大地减少了参数量和计算量。然后通过一个Sigmoid的门获得0~1之间归一化的权重，最后通过一个Scale的操作来将归一化后的权重加权到每个通道的特征上。
* SE-ResNet Module 操作过程基本和SE-Inception一样，只不过是在Addition前对分支上
Residual的特征进行了特征重标定。如果对Addition后主支上的特征进行重标定，
由于在主干上存在0~1的scale操作，在网络较深BP优化时就会在靠近输入层容易
出现梯度消散的情况，导致模型难以优化。

```
X1 = conv(X0)
X2 = weight(X0)
X3 = X0 + X1 ** X2
```

{{% /admonition %}}

### 6.2 环境参数

<<<<<<< HEAD
{{< admonition note "环境参数">}}
* Tensorflow1.12
* Ubuntu16.04
* Python3.5
{{< /admonition >}}

### 6.3 数据准备

{{< admonition note "数据准备">}}
=======
{{% admonition note "环境参数"%}}
* Tensorflow1.12
* Ubuntu16.04
* Python3.5
{{% /admonition %}}

### 6.3 数据准备

{{% admonition note "数据准备"%}}
>>>>>>> add / update posts
* 300W-LP
{{% /admonition %}}

## 7 实例代码

>
>正在优化中，后期上传
>
> -- _ieblYang_

## 8 参考资料

<<<<<<< HEAD
{{< admonition note "参考资料">}}
=======
{{% admonition note "参考资料"%}}
>>>>>>> add / update posts
* [人脸关键点对齐](https://www.jianshu.com/p/e4b9317a817f)
* [人脸对齐（十）--人脸对齐综述](https://blog.csdn.net/App_12062011/article/details/81777923?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
* [人脸特征点检测（四）——Tasks-Constrained DCN（TCDCN）](https://blog.csdn.net/qq_28618765/article/details/78128619?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
* [人脸关键点检测9——DAN](https://blog.csdn.net/u013841196/article/details/81048054)
* [SE-Inception v3架构的模型搭建（keras代码实现）](https://www.tinymind.cn/articles/3764)
* [对SE_ResNet的理解](https://blog.csdn.net/qq_22764813/article/details/95051082)
{{% /admonition%}}
