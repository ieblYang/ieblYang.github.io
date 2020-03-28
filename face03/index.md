# 卷积神经网基础串讲


基于TensorFlow的人脸识别智能小程序的设计与实现 卷积神经网基础串讲

<!--more-->
## 1 卷积神经网发展历程与基本概念

![Minion](/images/face/face03/1.jpg)

### 1.1 什么是卷积神经网

{{< admonition note "笔记" >}}
* 以卷积结构为主，搭建起来的深度网络
* 将图片作为网络的输入，自动提取特征，并对图片的变形（如平移、比例缩放、倾斜）等具有高度不变形
{{< /admonition >}}

## 2 卷积神经网络的重要组成单元

### 2.1 卷积

#### 2.1.1 基本定义

{{< admonition note "笔记">}}
* 基本定义：对图像和滤波矩阵做内积（逐个元素相乘再求和）的操作
	* 滤波器
	* 每一种卷积对应一种特征
	* lm2col实现卷积运算
{{< /admonition >}}

#### 2.1.2 卷集中的重要参数

##### 1. 卷积核

* 最常用为2D卷积核`（k_w * k_h）`
* 权重和偏置项

* 常用卷积核：`1*1`、`3*3`、`5*5`
	* 保护位置信息
	* padding时对称

##### 2. 卷积  

* 权值共享与局部连接（局部感受野/局部感知）
* 卷积运算作用在局部
* Feature map使用同一个卷积核运算后得到一种特征
* 多种特征采用多个卷积核（channel）

##### 3. 卷积核与感受野

* 如何计算卷积参数量？（Parameters）
	* `( k_w * k_h * In_channel + 1 ) * Out_channel`	
* 如何计算卷积的计算量？（FLOPs）
	* `In_w * In_h * ( k_w * k_h * In_channel + 1 ) * Out_channel`

##### 4. 步长

* 下采样过程
* 输出 Feature Map 的大小如何变化？
* 参数量和计算量？

##### 5. Pad

* 确保Feature Map整数倍变化，对尺度相关的任务尤为重要
* 参数量和计算量？

#### 2.1.3 卷积的定义与使用（caffe）

```Python
layer{
    name:"conv1"
    type:"Convolution"
    bottom:"data"
    top:"conv1"
    param{}
    convolution_param{}
}

param{
    lr_mult:1 //weight
}
param{
    lr_mult:2 //bias
}

convlolution_param{
    num_output:20
    kernel_size:5
    stride:1
    weight_filler{
        type:"xavier"}
    bias_filler{
        type:"constant"}    
    }
```

#### 2.1.4 卷积的定义与使用（TensorFlow）

```Python
filter_weight = tf.get_variable('weight',[5,5,3,16],
initializer = tf.truncated_normal_initializer(stddev=0.1))
biases = ft.get_variable('biases',[16],initializer = tf.constant_initializer(0.1))
conv = tf.nn.conv2d(input,filter_weight,strides=[1,1,1,1],padding='SAME')
bias = tf.nn.bias_add(conv,biases)

net = slim.conv2d(input_x,64,[3,3],scope='conv1_1')
```

### 2.2 池化

#### 2.2.1 基本定义

{{< admonition note "笔记">}}
* 基本定义：对输入的特征图进行压缩
	* 使特征图变小，简化网络计算复杂度
	* 进行特征压缩，提取主要特征
	* 增大感受野
{{< /admonition >}}

#### 2.2.2 常见的池化策略

* 最大池化（Max Pooling）
* 平均池化（Average Pooling）
* 随机池化（Stochastic Pooling）

### 2.3 激活

#### 2.3.1 基本定义

{{< admonition note "笔记">}}
* 基本概念：增加网络的非线性，进而提升网络的表达能力
	* 非线性
	* 单调性
	* 可微性
	* 取值范围
	* Sigmoid、Tanh、ReLU、ELU、Maxout、Softplus、Softsign
{{< /admonition >}}

#### 2.3.2 Sigmoid

* 梯度弥散/梯度饱和
* 指数运算
* 输出不是以零为中心

![Minion](/images/face/face03/2.jpg)

#### 2.3.3 Tanh

* 双曲正切函数（Tanh）
* 完全可微分的，反对称，对称中心在原点
* 指数运算

![Minion](/images/face/face03/3.jpg)

#### 2.3.4 ReLU

![Minion](/images/face/face03/4.jpg)

* 修正线性单元（Rectified Linear Unit，ReLU）
* 保留了step函数的生物学启发（只有输入超出阈值时神经元才激活）
* 函数形式简单，正数时不存在梯度饱和
* 一旦输入到了负数，ReLU就会死掉

### 2.4 BN（BatchNorm）

#### 2.4.1 基本概念

{{< admonition note "笔记">}}
* 基本概念：通过一定的规范化手段，把每层神经网络任意神经元这个输入值的分布强行拉回到均值为0方差为1的标准正态分布
![Minion](/images/face/face03/5.jpg)
{{< /admonition >}}

#### 2.4.2 BatchNorm层优点

* 减少了参数的人为选择，可以取消Dropout和L2正则项参数，或者采取更小的L2正则项约束参数；
* 减少了对学习率的要求
* 可以不再使用局部响应归一化，BN本身就是归一化网络（局部响应归一化——AlexNet）；
* 破坏原来的数据分布，一定程度上缓和过拟合。

#### 2.4.3 BatchNorm层使用

在Caffe中使用BN层需要注意： 
* 要配合Scale层一起使用
* 训练时use_global_stats设置为 false
* 测试时user_global_stats设置为 true

```Python
layer{
    bottom:"conv1"
    top:"conv1"
    name:"bn_conv1"
    type:"BatchNorm"
    batch_norm_param{
        use_global_stats:true
    }
}
```

### 2.5 全连接层（Fully Connected Layer）

连接所有的特征，将输出值分送给分类器（softmax分类器）
* 将网络的输出变成一个向量
* 可以采用卷积替代全连接层
* 全连接层是尺度敏感的
* 配合使用dropout层

### 2.6 Dropout层

在训练过程中，随机的丢弃一部分输入，此时丢弃部分对应的参数不会更新
* 数据过拟合问题
	* 取平均的作用
	* 减少神经元之间复杂的共适应关系 

![Minion](/images/face/face03/6.jpg)

### 2.7 损失层（LOSS）

#### 2.7.1 损失函数

损失函数：用来评估模型的预测值和真实值的不一致程度。
* 经验风险小
* 结构风险小
* 交叉熵损失、softmax loss等

![Minion](/images/face/face03/7.jpg)

#### 2.7.2 损失层

损失层定义了使用的损失函数，通过最小化损失来驱动网络的训练
* 网络的损失通过前项操作计算
* 网络参数相对于损失函数的梯度则通过反向操作计算
	* 分类任务损失：交叉熵损失
	* 回归任务损失：L1损失、L2损失

#### 2.7.3 交叉熵损失

* log-likelihood cost
* 非负性
* 当真实输出a与期望输出y接近的时候，代价函数接近于0

![Minion](/images/face/face03/8.jpg)

```Python
# 交叉熵损失实现
tf.nn.softmax_cross_entrpoy_with_logits(_sentinel=None,labels=None,
logits=None,dim=-1,name=None)

tf.nn.sparse_softmax_cross_entrpoy_with_logits(_sentinel=None,labels=None,
logits=None,name=None)
```

* L1、L2、Smooth L1损失
	* Smooth L1是L1的变形，用于Faster RCNN、SSD等网络计算损失
	![Minion](/images/face/face03/9.jpg)

## 3 常见的卷积神经网结构

### 3.1 LeNet

麻雀虽小五脏俱全
* 1998年，LeCun提出
* 用于解决手写数字识别，MNIST

### 3.2 AlexNet

#### 3.2.1 概念

* 2012年，Hinton的学生Alex Krizhevsky 做出DeepLearning模型。
* con - relu - pooling - LRN
* fc - relu - dropout
* fc - softmax
* 参数量60M以上
* 模型大小>200M 

![Minion](/images/face/face03/10.jpg)

#### 3.2.2 AlexNet的特点

* ReLU非线性激活函数
* Dropout层防止过拟合
* 数据增强，减少过拟合
* 标准化层（Local Response Normalization）

#### 3.2.3 AlexNet的意义

* 证明了CNN在复杂模型下的有效性
* GPU实现使得训练在可接受的时间范围内得到结果

### 3.3 ZFNet

* 在AlexNet基础上进行细节调整，并取得2013年ILSVA的冠军
	* 从可视化的角度出发，解释CNN有非常好的性能的原因
* ZFNet与特征可视化
	* 特征分层次体系结构
	* 深层特征更鲁棒
	* 深层特征更有区分度
	* 深层特征收敛更慢
	* ......  

![Minion](/images/face/face03/11.jpg)	

### 3.4 VGGNet

#### 3.4.1 概念

* 由牛津大学计算机视觉组和Google Deepmind共同设计
* 为了研究网络深度对模型准确度的影响，并采用小卷积堆叠的方式来搭建整个网络结构
* 参数量：138M
* 模型大小>500M  

![Minion](/images/face/face03/12.jpg)

#### 3.4.2 VGGNet的特点

* 更深的网络结构，结构更加规整、简单；
* 全部使用3*3的小型卷积核和2*2的最大池化层
* 每次池化后Feature Map宽高降低一半，通道数量增加一倍；
* 网络层数更多、结构更深，模型参数量更大。

#### 3.4.3 VGGNet的意义

* 证明了更深的网络，能够提取更好的特征
* 成为后续很多网络的backbone
* 规范化了后续网络设计的思路

### 3.5 GoogLeNet/Inception v1

#### 3.5.1 概念
* 在设计网络结构时，不仅强调网络的深度，也会考虑网络的宽度，并将这种结构定义为Inception结构（一种网中网（Network In Network）的结构，及原来的结点也是一个网络）
* 证明了用更多的卷积，更深的层次可以得到更好的结构
* 参数量6.8M
* 模型大小50M  

![Minion](/images/face/face03/13.jpg)

#### 3.5.2 GoogLeNet/Inception v1特点

* 更深的网络结构
* 两个LOSS层，降低过拟合风险
* 考虑网络宽度
* 巧妙地利用`1*1`的卷积核来进行通道降维，减小计算量

#### 3.5.3 从卷积的角度思考，如何减少网络中的计算量？

* 小卷积核来对大卷积核进行拆分
* Stride = 2代替pooling层
* 巧妙地利用`1*1`的卷积核来进行

### 3.6 ResNet

#### 3.6.1 概念

* 2015年，由何凯明团队提出，引入跳连的结构来防止梯度消失的问题，进而可以进一步加大网络深度。

#### 3.6.2 ResNet中的Bootleneck与恒等映射

* Bootleneck：跳连结构（Short-Cut）恒等映射，解决梯度消失问题；

#### 3.6.3 ResNet中的BatchNorm

* 每个卷积之后都会配合一个BatchNorm层
* 对数据scale和分布来进行约束
* 简单的正则化，提高网络抗过拟合能力

#### 3.6.4 ResNet的设计特点

* 核心单元简单堆叠
* 跳连结构解决网络梯度消失问题
* Average Pooling层代替FC层
* BN层加快网络训练速度和收敛时的稳定性
* 加大网络深度，提高模型特征提取能力

#### 3.6.5 ResNet的变种网络

* ResNetXt	分组卷积
* DenseNet	更多的跳连
* Wide-ResNet	加大网络宽度
* ResNet In ResNet	网中网
* Inception-ResNet	Inception结构

## 4 轻量型卷积神经网

{{< admonition note "笔记">}}
* 更少的参数量
* 更少的计算量
* 移动端、嵌入式平台
{{< /admonition >}}

### 4.1 SqueezeNet

* LCLR-2017，作者分别来自Berkeley和Stanford
	* 提出 Fire Module，由两部分组成：Squeeze 层 + Expand层
* SqueezeNet的特点
	* `1*1`卷积核减小计算量
	* 不同size的卷积核，类似Inception
	* deep compression技术

### 4.2 MobileNet V1/V2

* 由Google团队提出，并发表于CVPR-2017
	* Depth-wise Separable Convolution 的卷积方式代替传统卷积方式，以达到减少网络权值参数的目的。
* MobileNet 网络设计思想
	* Depth-wise Convolution
	* Point-wise Convolution
* MobileNet V2
	* Inverted residuals（倒置残差）
	* Linear bottlenecks  

![Minion](/images/face/face03/14.jpg)

### 4.3 ShuffleNet V1/V2

* ShuffleNet V1由旷视科技提出的一种轻量型卷积网络
	* 深度卷积来代替标准卷积
	* 分组卷积+通道shuffle
* ShuffleNet V2旷视科技针对ShuffleNet V1改进的轻量型卷积神经网
	* ECCV 2018
	* 该模型最大的贡献点在于解释了如何去设计轻量型卷积网络的几个标准和规范

{{< admonition note "笔记">}}
**轻量型卷积神经网设计标准**
* 相同的通道宽度可最小化内存访问成本（MAC）
* 过度的组卷积会增加MAC
* 网络碎片化（例如GoogleNet的多路径结构）会降低并行度
* 元素级运算不可忽视
{{< /admonition >}}

### 4.4 Xception

* 由Google提出，arXiv的V1版本于2016年10月公开
* 同样借鉴了深度卷积的思想，但又存在差异，具体如下：
* Xception先采用1*1卷积，再进行主通道卷积；
* Xception再1*1卷积后，加入ReLU；

## 5 多分支卷积神经网络

### 5.1 Siamese Net

* 孪生网络
* 余弦距离
	* 余弦距离，也称余弦相似度，是用向量空间中两个向量夹角的余弦值作为衡量两个个体间差异大小的度量
	![Minion](/images/face/face03/15.jpg)
* 余弦距离——拓展Loss
	* Center Loss
	* SphereFace
	* CosFace
	* ArcFace
	* CCL
	* AMSoftmax
* 度量问题
	* 分类问题
	* 回归问题
	* 度量问题
		* 相似度
		* 排序问题

### 5.2 Troplet Net

* Anchor + Negative + Positive

![Minion](/images/face/face03/16.jpg)

* Troplet Net 网络结构

![Minion](/images/face/face03/17.jpg)

* Troplet Net 网络特点
	* 提取Embedding Feature
	* 细粒度的识别任务
	* 正负样本比例失衡——难例挖掘

### 5.3 Quadruplet Net

* 相比Troplet Net 多加入一张负样本
* 正度样本之间的绝对距离

### 5.4 多任务网络

![Minion](/images/face/face03/18.jpg)

## 6 卷积神经网中的Attention机制

{{< admonition note "笔记">}}
人类大脑在接受和处理外界信号时的一种机制
* one-hot分布或者soft的软分布
* Soft-Attention或者Hard-Attention

![Minion](/images/face/face03/19.jpg)

{{< /admonition >}}

Attention实现机制：
* 保留所有分量均做加权（即soft attention）
* 在分布中以某种采样策略选取部分分量（即hard attention）
	* 原图、特征图、空间尺度、通道、特征图上的每个元素、不同历史时刻
	* ResNet + Attention

	![Minion](/images/face/face03/20.jpg)

	* SENet/Channel Attention

	![Minion](/images/face/face03/21.jpg)

## 7 模型压缩

{{< admonition note "笔记">}}
学院派VS工程派
* 学院派注重精度
* 工程派注重精度与效率的结合
{{< /admonition >}}

### 7.1 模型剪枝

* 除无意义的权重和激活来减少模型的大小
	* 贡献度排序
	* 去除小贡献度单元
	* 重新fine-tuning
* 模型剪枝技巧
	* 全连接部分通常会存在大量的参数冗余
	* 对卷积窗口进行剪枝的方式，可以使减少卷积窗口权重，或者直接丢弃掉卷积窗口的某一维度；
	* 丢弃稀疏的卷积窗口，但这并不会使模型运行速度有数量级的提升；
	* 首先训练一个较大的神经网络模型，在逐步剪枝得到的小模型。

### 7.2 模型量化/定点化

* 减少数据在内存中的位数操作，可以采用8位类型来表示32位浮点（定点化）或者直接训练低于8位的模型，比如：2bit模型、4bit模型等
	* 较少内存开销，节省更多的带宽
	* 对于某些定点运算方式，甚至可以消除乘法操作，只剩加法操作，某些二值模型，直接使用位操作
* 代价通常是位数越低，精度下降越明显
	* 在TensorFlow中，通常采用引入量化层的方式来更改计算图，进而达到量化的目的。

![Minion](/images/face/face03/22.jpg)

### 7.3 知识蒸馏（Knowledge Distillation）

采用一个大的、复杂的网络模型来指导一个小的、精简之后的网络模型进行模型训练和学习

![Minion](/images/face/face03/23.jpg)

