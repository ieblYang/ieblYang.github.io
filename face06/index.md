# 人脸检测业务理论


基于TensorFlow的人脸识别智能小程序的设计与实现 人脸检测业务理论

<!--more-->

## 1 人脸业务场景综述

### 1.1 生物信息识别

* 人脸识别是生物信息识别领域重要研究方向之一
* 生物特征有手形、指纹、脸形、虹膜、视网膜、脉搏、耳廓等

### 1.2 人脸业务场景

#### 1.2.1 人脸检测（Face Detection）

* 检测出图像中人脸所在位置的一项技术

* 基础提供支持
	* 姿态和表情的变化
	* 不同人的外观差异
	* 光照、遮挡、视角
	* 不同大小、位置  

![Minion](/images/face/face06/1.jpg)

#### 1.2.2 人脸关键点（人脸对齐 Face Alignment）

* 定位出人脸上五官等关键点坐标的一项技术

* 人脸美颜、活体检测等基础
	* 人脸外观差异
	* 角度、姿态、遮挡等问题
	* 实时性
	* 稠密关键点（>68点）  

![Minion](/images/face/face06/2.jpg)

#### 1.2.3 人脸属性（Face Attribute）

* 识别出人脸的性别、年龄、姿态、表情等属性值的一项技术

* 人脸分析基础
	* 人脸外观差异
	* 角度、姿态、遮挡等问题
	* 等等  

![Minion](/images/face/face06/3.jpg)

#### 1.2.4 人脸对比（Face Compare）

计算两个人脸之间的相似度
* 人脸验证（Face Verification)
* 人脸识别（Face Compere）
* 人脸检索（Face Retrieval）
* 人脸聚类（Face Cluster）
* 等等  

![Minion](/images/face/face06/4.jpg)

#### 1.2.5 其他人脸业务场景

* 人脸活体检测
* 情绪识别
* 面相分析
* 颜值评分
* 明星脸匹配
* 人脸生成
* 人脸美妆、美颜
* 人脸风格化
* 痘痘、皱纹检测
* 人脸换脸

### 1.3 人脸业务流程  

![Minion](/images/face/face06/5.jpg)

### 1.4 人脸采集相关问题

* 不同性别分布，男性、女性。
* 不同年龄分布，儿童、少年、中年、老年。
* 不同人种分布，黑人、白人、黄种人。.
* 不同脸型分布，人脸、猪脸、猴脸。
* 人脸没有正对摄像头，角度有倾斜，左右倾斜、. 上下倾斜。
* 翻拍的人脸照片，清晰照片、不清晰照片。
* 摄像头内包含单张人脸、多张人脸。
* 测试所处的环境:光线正常、过亮、过暗、暖光、冷光、白平衡等
* 不同场景:室内、室外、车站、超市等

### 1.5 人脸资源——相关数据集

* PubFig: Public Figures Face Database(哥伦比亚大学公众人物脸部数据库)
* **Large-scale CelebFaces Attributes (CelebA) Dataset**
* Colorferet
* Multi-Task Facial Landmark (MTFL) dataset
* BiolD Face Database - FaceDB
* **Labeled Faces in the Wild Home (LFW)**
* Person identification in TV series
* CMUVASC & PIE Face dataset
* YouTube Faces
* **Wider-face**
* MegaFace
* CASIA-FaceV5
* The CNBC Face Database
* CASIA-3D FaceV1
* IMDB-WIKI
* FDDB 
* Caltech人脸数据库
* The Japanese Female Facial Expression (JAFFE) Database
* https://blog.csdn.net/perception/article/details/80656186>

### 1.6 人脸资源——相关厂商

![Minion](/images/face/face06/6.jpg)

## 2 人脸检测问题

* 什么是人脸检测问题？
	* 从图像中找到人脸的位置

* 人脸检测的用处？
	* 更准确的人脸业务
		* 人脸识别
		* 人脸对比
		* 人脸关键点
		* 人脸属性  
		
![Minion](/images/face/face06/7.jpg)

## 3 人脸标注方法

### 3.1 矩形框

传统方法都采用矩形标记法，用一个矩形框将画面中的人脸区域包含在内。这种标记方法存在的问题在于很难给出一个恰好包含面部的矩形框，并且获得各种不同算法的一致的认可。因此采用矩形框的方法无法很好的对不同算法的结果做出准确且有效的评价。

* 常用四个值来标定(x,y,w,h)  
* (x,y)表示矩形左上角的坐标，w 矩形的宽，h 矩形的高
* 优点：网络学习起来更容易，目前很多人脸检测算法框架也采用矩形框标注的方法。
* 缺点：标注的区域中一部分并不属于人脸。 

![Minion](/images/face/face06/8.jpg)

### 3.2 椭圆形标注

由于人脸天然呈现为椭圆形，所以用椭圆形来表征是一种较为准确的方法，如下图所示,这种方法可以对侧脸与转动后的面部进行描述，常用5个值来标定。

* ra：椭圆长轴半径
* rb：椭圆短轴半径
* theta：椭圆长轴偏转角度
* cx：椭圆圆心x坐标
* cy：椭圆圆心y坐标  

![Minion](/images/face/face06/9.jpg)	

## 4 人脸检测性能评价指标

### 4.1 检测率、误报率

* 检测率 = 正确检出人脸个数/真实人脸个数
* 误报率 = 检出目标中错误的人脸数量/总共检测出的目标数量
* 每一个[标记]^(真值:GT)只允许有一个检测与之相对应
* 重复检测会被视为错误检测

### 4.2 速度：FPS

### 4.3 IOU(Intersection-over-Union)  

* $IOU = \dfrac{area(C) \bigcap area(G)}{area(C) \bigcup area(G)}$  
* 产生的候选框（candidate bound）与原标记框（ground truth bound）的交叠率,即它们的交集与并集的比值。
* 最理想情况是完全重叠，即比值为1。  

![Minion](/images/face/face06/10.jpg)

### 4.4 Precision-Recall/ROC

* accuracy $ =\frac{TP}{P+N}$  
  
|符号 |         含义         |
| :-: | :------------------: |
|  p  | Positive 正样本数量  |
|  n  | Negative 负样本数量  |
|  Y  | 预测为正样本数量     |
|  N  | 预测为负样本数量     |
  
![Minion](/images/face/face06/11.jpg)

#### 4.4.1 PR曲线

* precisidon$ =\frac{TP}{TP+FP}$
* recall$ = \frac{TP}{P}$ 
   
|符号 |以人脸识别（PR）为例|
| :-: | :----------------: |
|  p  |    真实人脸区域    |
|  n  |    真实背景区域    |
|  Y  |    检出的人脸框    |
|  N  |    没有检出区域    |

* 11-point interpolated average precision  

![Minion](/images/face/face06/12.jpg)

#### 4.4.2 ROC曲线

* 前提：当前图片智能有一个人脸
* 通常用于FDDB数据集
* fp&nbsp;rate$ =\frac{FP}{N}$
* tp&nbsp;rate $= \frac{TP}{P}$  
  
|     |    以人脸识别（ROC）为例    |
| :-: | :-------------------------: |
|  p  |  图片中存在人脸的图片       |
|  n  |  图片中不存在人脸的图片     |
|  Y  |  在当前图片中检测出人脸     |
|  N  |  在当前图片中没有检测出人脸 | 
   
![Minion](/images/face/face06/11.1.jpg)

## 5 人脸检测方法介绍

### 5.1 传统的人脸检测方法  

![Minion](/images/face/face06/13.jpg)

{{< admonition note "笔记">}}
* VJ(Viola-Jones)框架算法要点:
	* 使用类Haar输入特征：对矩形图像区域的和或差进行阈值化。
	* 积分图像技术加速了矩形图像区域的45度旋转的值的计算，这个图像结构被用来加速类Haar输入特征的计算。
	* 使用Adaboost来创建二分类问题（人脸与非人脸）的分类器节点（高通过率，低拒绝率）。
	* 把分类器节点组成筛选式级联（在筛选式级联里，一个节点是Adaboost类型的一组分类器）。

* DPM
	* DPM算法由Felzenszwalb提出，是一种基于部件的检测方法，
	对目标的形变具有很强的鲁棒性。目前DPM已成为众多分类、分割、姿态估计等
	算法的核心部分，Felzenszwalb本人也因此被VOC授予"终身成就奖"。
	* DPM算法采用了改进后的HOG特征，SVM分类器和滑动窗口（Sliding Windows）检测思想，
	针对目标的多视角问题，采用了多组件（Component）的策略，针对目标本身的形变问题，
	采用了基于图结构（Pictorial Structure）的部件模型策略。
	此外，将样本的所属的模型类别，部件模型的位置等作为潜变量（Latent Variable），
	采用多示例学习（Multiple-instance Learning）来自动确定。

* JDA（Joint Cascade Face Detection and Alignment）
	* 目前比较先进的人脸检测算法.它结合了 cascade 和 alignment ，一方面做alignment对进一步的人脸识别意义重大，
	另一方面作者在 section 2 讲到了landmark附近的特征可促进分类器分辨出更准确的结果，
	最后，将这两者放在一起做不仅相互促进而且还相互节省了时间。
{{< /admonition >}}

### 5.2 从粗粒度到细粒度的级联模型（Deep Learning） 

![Minion](/images/face/face06/14.jpg)

### 5.3 通用目标检测算法+基于人脸问题的优化

* 通用目标检测算法  

![Minion](/images/face/face06/15.jpg)

* 人脸检测算法  

![Minion](/images/face/face06/16.jpg)

>
>人脸检测相关文献：
><http://www.imooc.com/article/284277>
>

## 6 人脸检测问题挑战及解决思路

* 人脸可能出现在图像中的任何一个位置
* 人脸可能有不同的大小
* 人脸在图像中可能有不同的视角和姿态
* 人脸可能部分被遮挡

## 7 小人脸检测问题/策略

### 7.1 小人脸检测问题

* 下采样倍率很大时，人脸区域基本消失
* 相比于感受野和anchor的尺寸来说，人脸的尺寸太小
* Anchor匹配策略（IOU小且变化敏感）
* 正负样本比例失衡  

![Minion](/images/face/face06/17.jpg)

### 7.2 小人脸检测问题策略

* 尺度不敏感/多尺度的策略
* 调整优化Anchor策略
* 在线的难例挖掘
* IOU计算方式
* 数据增强

## 8 SSD模型 

### 8.1 Single Shot MultiBox Detector(one-stage方法)

* Wei Liu在ECCV 2016提出
* 端到端的训练
* 直接回归目标类别和位置
* 不同尺度的特征图上进行预测

### 8.2 SSD模型介绍  

![Minion](/images/face/face06/18.jpg)

* 主干网络：VGGNet
* 多尺度Feature Map预测
* Default bounding boxes的类别分数、偏移量

#### 8.2.1 Anchor 

Anchor指特征图上的每一个点

![Minion](/images/face/face06/19.jpg)

#### 8.2.2 Default box

* `m*n`个单元，每个单元称作一个Anchor

* 每个单元上生成固定尺度和长宽比的box
	* 假设一个特征图有mxn个单元，每个单元对应k个default box,每个default box预测c个类别概率分布和4个坐标
	* `(c+4)*k*m*n`个输出值

#### 8.2.3 Prior box

* 实际选择的default box
* `38*38*4 + 19*19*6 + 10*10*6 + 5*5*6 + 3*3*4+1*1*4= 8732`个prior box

#### 8.2.4 损失函数  

$$L(x,c,l,g)=\dfrac{1}{N} (L_{conf}(x,c)+\alpha L_{loc}(x,l,g))$$

#### 8.2.5 样本构造

* 正样本
	* 从GT box出发给找到最匹配的prior box放入候选正样本集
	* 从prior box集出发，寻找与GT box满足lOU> 0.5的最大prior box放入候选正样本集

* 负样本
	* 难例挖掘
	* 正负样本比 1:3

#### 8.2.6 数据增强

* 随机采样多个path,与物体之间最小的`jaccard overlap`为: 0.1, 0.3, 0.5, 0.7与0.9
* 采样的区域比例是[0.3, 1.0]， aspect ratio 在0.5或2
* GTbox中心在采样区域中且面积大于0
* Resize到固定大小
* 以0.5的概率随机的水平翻转

## 8.3 TensorFlow+SSD环境搭建

{{< admonition note "笔记">}}
* 安装TensorFlow环境
	* TensorFlow-gpu版本1.12以上

* https://github.com/tensorflow/models.git
   * [目标检测算法源码框架](https://github.com/tensorflow/models/tree/master/research/object_detection)
   * [安装教程](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md)
{{< /admonition >}}

## 9 人脸检测数据清洗与数据打包

### 9.1 WIDER FACE 数据集

{{< admonition note "笔记">}}
WIDER FACE数据集
* 由香港中文大学提出
* 32203个图像和393703个人脸图像
* 在尺度，姿势，遮挡，表情，装扮，光照等
* 61个事件类别组织的，对于每一个事件类别，选取其中的40%作为训练集,10%用于交叉验证，50%作为测试集 
* 下载地址：<http://shuoyang1213.me/WIDERFACE/> 
{{< /admonition >}}

![Minion](/images/face/face06/20.jpg)

### 9.2 Passcal VOC

* 对于WIDER FACE数据集，我们首先会将他转化为Passcal VOC格式  
![Minion](/images/face/face06/21.jpg)
* VOC主要有三个重要的文件夹

{{< admonition note "笔记">}}
* Annotations 存放标注信息
* ImageSets	  存放训练和测试用到的文件列表
* JPEGImages  存放图片信息
{{< /admonition >}}

* VOC数据基本结构

```
<annotaion>
	<floder>widerface</floder>
	<filename>0--Parade_0_Parade_marchingband_1_5.jpg</filename> #指向了JPEGImage下面的一个图片
	<source> #图片来源
		<databse>wider face Database</databse>
		<annotation>PASCAL VOC2007</annotaion>
		<image>flickr</image>
		<flickrid>1</flickerid>
	<source>
	<owner>
		<flickerid>ieblyang</flickerid>
		<name>ieblyang</name>
	</owner>
	<size> #当前图片的尺寸，通道数
		<width>1024</width>
		<height>683</height>
		<depth>3</depth>
	</size>
	<segmented>0</segmented>
	<object> #一个object对应一个人脸
		<name>face</name>
		<pose>Unspecified</poss>
		<truncated>1</truncated>
		<diffcult>0</diffcult>
		<bndbox>#记录了矩形框的坐标值
			<xmin>495</xmin>
			<ymin>347</ymin>
			<xmax>532</xmax>
			<ymax>398</ymax>
		</bndbox>
	</object>
...
</annotaion>		   
```

### 9.3 WIDER FACE 标注文件

``` 
# wider_face_train_bbx_gt.txt
0--Parade/0_Parade_marchingband_1_849.jpg   # 图片路径
1                                           # 针对当前图片中的人脸个数
449 330 122 149 0 0 0 0 0 0                 # 当前人脸的标准信息
↑   ↑
前两个值人脸框左上角坐标
        ↑   ↑
        第三四个值表示人脸框的大小
                ↑ ↑ ↑ ↑ ↑ ↑
                后六个值分别表示Scale、Pose、Occlusion、Expression、Makeup、Illumation
                利用这六个值可以对人脸数据进行清洗
```

### 9.4 实例代码

>处理widerface数据集转VOC
><https://github.com/ieblYang/widerface>
>
> -- _ieblYang_

## 10 参考资料

{{< admonition note "参考资料">}}
* [FDDB人脸检测算法评价标准](https://yinguobing.com/fddb/)
* [目标检测 IOU（交并比） 理解笔记](https://www.cnblogs.com/zfcode/p/mu-biao-jian-ce-IOU-jiao-bing-bi-li-jie-bi-ji.html)
* [走近人脸检测（2）——VJ人脸检测器及其发展](https://blog.csdn.net/zchang81/article/details/71515490)
* [Viola-Jones人脸检測](https://www.cnblogs.com/llguanli/p/8918677.html)
* [DPM目标检测算法](https://blog.csdn.net/weixin_41798111/article/details/79989794)
* [解读 Joint Cascade Face Detection and Alignment 人脸检测算法](https://blog.csdn.net/smf0504/article/details/52244254)
* [人脸识别技术大总结1——Face Detection & Alignment](https://www.cnblogs.com/sciencefans/p/4394861.html)
{{< /admonition>}}
