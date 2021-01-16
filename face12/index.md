# 人脸属性业务实战


基于TensorFlow的人脸识别智能小程序的设计与实现 人脸属性业务实战

<!--more-->

## 1 人脸属性业务介绍

### 1.1 什么是人脸属性

* 人脸属性指的是根据给定的人脸判断其性别、年龄和表情等
* 分类问题、回归问题
* 性别、是否戴眼镜、年轻人、微笑  

![Minion](/images/face/face12/1.jpg)

### 1.2 常用数据集

|常用数据集|
|:---:|
|UTKFace|
|SCUT-FBP5500|
|CelebA|
|APPA-REAL|
|AFAD Dataset|
|FER+|
|NKI|

## 2 人脸属性方法介绍

![Minion](/images/face/face12/2.jpg)

* 离散值：性别，戴眼镜，戴面纱，种族，表情等
* 连续值：年龄等

## 3 人脸属性方法难点

* 数据：不同人种，不同年龄，不同性别等等 
* 林志颖、郭德纲年龄差别不大，但从面部很难区分 

![Minion](/images/face/face12/3.jpg)

## 4 数据准备

* [CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)
* CelebA是香港中文大学的开放数据，包含10177个名人身份的202599张图片
* 40个属性

### 4.1 数据下载

* [Google Drive](https://drive.google.com/open?id=0B7EVK8r0v71pWEZsZE9oNnFzTm8)
* [Baidu Drive](https://pan.baidu.com/s/1eSNpdRG#list/path=%2F)

#### 4.1.1 说明

* 下载后会得到三个文件夹和一个 README.txt  

![Minion](/images/face/face12/5.jpg)  

CelebA中有 10,177人 的 202,599张 图片  
├── Anno  
│   ├── identity_CelebA.txt  
│   ├── list_attr_celeba.txt  
│   ├── list_bbox_celeba.txt  
│   ├── list_landmarks_align_celeba.txt  
│   └── list_landmarks_celeba.txt  
├── Eval  
│   └── list_eval_partition.txt  
├── Img  
│   ├── img_celeba.7z  
│   ├── img_align_celeba.zip 
│   └── img_align_celeba_png.7z 
└── README.txt  

{{% admonition note "笔记"%}}
* `identity_CelebA.txt`	身份标记：图片名 + 编号，相同编号代表同一人，如：000001.jpg 2880
* `list_attr_celeba.txt`  标记图片属性  
* `list_landmarks_align_celeba.txt`  对齐后的图片，人脸标记（眼鼻嘴）`lefteye_x` `lefteye_y` `righteye_x` `righteye_y` `nose_x` `nose_y` `leftmouth_x` `leftmouth_y` `rightmouth_x` `rightmouth_y`  
* `list_landmarks_celeba.txt` 自然环境下的图片，人脸标记（眼鼻嘴）`lefteye_x` `lefteye_y` `righteye_x` `righteye_y` `nose_x` `nose_y` `leftmouth_x` `leftmouth_y` `rightmouth_x` `rightmouth_y`  
* `list_eval_partition.txt`  分组：训练、验证、测试  
{{% /admonition %}}

#### 4.1.2 下载的图片

* `img_celeba.7z`：纯“野生”文件，也就是从网络爬取的没有做裁剪的图片
* `img_align_celeba.zip`：jpg格式的，比较小（推荐使用，直接解压即可
* `img_align_celeba_png.7z`：把“野生”文件裁剪出人脸部分之后的图片，png格式

#### 4.1.3 list_attr_celeba.txt中属性说明

|英文|中文|
|---|---|
|5_o_Clock_Shadow|胡须|
|Arched_Eyebrows|柳叶眉|
|Attractive|有魅力|
|Bags_Under_Eyes|眼袋|
|Bald|秃顶|
|Bangs|刘海|
|Big_Lips|大嘴唇|
|Big_Nose|大鼻子|
|Black_Hair|黑发|
|Blond_Hair|金发|
|Blurry|模糊|
|Brown_Hair|棕色头发|
|Bushy_Eyebrows|浓眉|
|Chubby|圆脸|
|Double_Chin|双下巴|
|Eyeglasses|戴眼镜|
|Goatee|山羊胡子|
|Gray_Hair|白发|
|Heavy_Makeup|浓妆|
|High_Cheekbones|高颧骨|
|Male|男人|
|Mouth_Slightly_Open|嘴微微张开|
|Mustache|胡子|
|Narrow_Eyes|小眼睛|
|No_Beard|没有胡须|
|Oval_Face|鸭蛋脸|
|Pale_Skin|苍白的皮肤|
|Pointy_Nose|尖鼻子|
|Receding_Hairline|	发际线高|
|Rosy_Cheeks|红润的脸颊|
|Sideburns|鬓胡|
|Smiling|微笑|
|Straight_Hair|直发|
|Wavy_Hair|卷发|
|Wearing_Earrings|戴着耳环|
|Wearing_Hat|戴着帽子|
|Wearing_Lipstick|擦口红|
|Wearing_Necklace|戴着项链|
|Wearing_Necktie|戴着领带|
|Young|年轻|

### 4.2 数据处理
对`img_celeba.7z.001~img_celeba.7z.014`这14个文件进行合并，得到一个`img_celeba.7z`文件，并解压
```Bash
# 数据合并
cat img_celeba.7z.0** > img_celeba.7z
# 安装 7z
sudo apt-get install p7zip-full 
# 解压
7z x img_celeba.7z 
```

## 5 多任务网络结构

![Minion](/images/face/face12/4.jpg)

## 6 实例代码

### 6.1 数据集处理

人脸属性的标注信息存储在Anno文件夹下的list_attr_celeba.txt中，从标注文件中可以看出，CelebA数据集中标注了40种人脸属性的信息，系统选取其中的四个属性进行数据打包，分别为是否戴眼镜、性别、是否微笑、是否年轻，这四个属性对应的下标依次为15、20、31、39。

在进行数据打包前，需要先对人脸图片进行适当的处理，在CelebA数据集中，所有的人脸图片存储在Img文件夹下，所有人脸图片又分为三类文件，分别是img_celeba.7z，纯“野生”文件，即从网络上爬取的没有进行裁剪的图片，img_algin_celeba_png.7z，将爬取到的“野生”文件裁剪出人脸部分后的到的图片，并且图片的格式为png格式，img_align_celeba.zip,将爬取到的“野生”文件经过人脸对齐和裁剪后得到的人脸图像，图片的格式为jpg格式，图片比较小，所以系统选用了img_algin_celeba文件中的图片进行处理和模型训练。

在数据集处理中，使用dlib库定义人脸检测器，对数据集中的人脸图片进行人脸检测，若没有发现人脸信息则将这张图片过滤掉，在成功获取到人脸框之后，需要对人脸框进行一定的扩充，对扩充后的人脸框仍然小于50 * 50的人脸图片要将其过滤掉，对数据集中的图片进行过滤的代码如下：

```Python
if len(rects) == 0:
        continue
    x1 = rects[0].left()
    y1 = rects[0].top()
    x2 = rects[0].right()
    y2 = rects[0].bottom()
    y1 = int(max(y1 - 0.3 * (y2 - y1), 0))
    if y2 - y1 < 50 or x2 - x1 < 50 or x1 < 0 or y1 < 0:
        continue
```

对符合要求的人脸图片，先进行人脸对齐，将图片裁剪为128 * 128的大小，然后定义tfrecord字典的格式，其中image为byte型，eyeglasses、male、smiling、young为int型，相关代码如下：

```Python
ex = tf.train.Example(
        features = tf.train.Features(
            feature = {
                "image": tf.train.Feature(
                    bytes_list = tf.train.BytesList(value=[im_data.tobytes()])
                ),
                "Eyeglasses": tf.train.Feature(
                    int64_list = tf.train.Int64List(
                        value = [int(attr_val[16])]
                    )
                ),
                "Male": tf.train.Feature(
                    int64_list = tf.train.Int64List(
                        value = [int(attr_val[21])]
                    )
                ),
                "Smiling": tf.train.Feature(
                    int64_list=tf.train.Int64List(
                        value=[int(attr_val[32])]
                    )
                ),
                "Young": tf.train.Feature(
                    int64_list = tf.train.Int64List(
                        value = [int(attr_val[40])]
                    )
                )
            }
        )
    )
```    
最后，对数据集进行训练集与测试集的划分，其中95%的数据划分为训练集，剩余的5%划分为测试集。

### 6.2 模型训练

主干网络使用slim提供的inception_v3_base，batch_norm层的参数配置如下：

```Python
batch_norm_params = {
        "is_training": is_training,
        "trainable": True,
        "decay": 0.9997,
        "epsilon": 0.00001,
        "variables_collections":{
            "beta": None,
            "gamma": None,
            "moving_mean":["moving_vars"],
            "moving_variance": ['moving_var']
        }
    }
```

对卷积层和全连接层进行权值上的约束时，正则化方法采用L2正则，并定义这些层是可训练的，对卷积层的参数进行初始化，调用主干网络即inception_v3_base传入图片，获取到inception_v3_base网络的输出结果，使用tf.reduce_mean函数计算张量tensor沿着1、2维度上的平均值。使用tf.nn.dropout函数在训练过程中随机的扔掉一部分神经元，防止或减轻过拟合问题，针对系统研究的四个属性以主干网络的输出为四个分支的输入来定义分支，主要代码如下：

```Python
weights_regularizer = tf.contrib.layers.l2_regularizer(0.00004)
    with tf.contrib.slim.arg_scope(
        [tf.contrib.slim.conv2d, tf.contrib.slim.fully_connected],
        weights_regularizer = weights_regularizer,
        trainable = True):
        with tf.contrib.slim.arg_scope(
            [tf.contrib.slim.conv2d],
            weights_initializer = tf.truncated_normal_initializer(stddev=0.1),
            activation_fn = tf.nn.relu,
            normalizer_fn = batch_norm,
            normalizer_params = batch_norm_params):
            nets, endpoints = inception_v3_base(images)
            print(nets)
            print(endpoints)
            net = tf.reduce_mean(nets,axis=[1,2])
            net = tf.nn.dropout(net, drop_out, name = "droplast")
            net = flatten(net, scope="flatten")
    net_eyeglasses = slim.fully_connected(net, 2, activation_fn = None)
    net_male = slim.fully_connected(net, 2, activation_fn=None)
    net_smiling = slim.fully_connected(net, 2, activation_fn=None)
    net_young = slim.fully_connected(net, 2, activation_fn=None)
    return net_eyeglasses, net_male, net_smiling, net_young
```

输入和label定义：网络搭建完成后，定义输入和label，并获取网络的预测结果，对于模型训练设置dropout=0.5，is_training=True，代码如下：
```Python
input_x = tf.placeholder(tf.float32, shape=[None, 128, 128, 3])
label_eyeglasses = tf.placeholder(tf.int64, shape=[None, 1])
label_male = tf.placeholder(tf.int64, shape=[None, 1])
label_smiling = tf.placeholder(tf.int64, shape=[None, 1])
label_young = tf.placeholder(tf.int64, shape=[None, 1])
```

Loss的定义：采用交叉熵损失作为模型训练的Loss，对于tf.losses.sparse_softmax_cross_entropy其中labels是一维的， 预测结果是二维的，最后结果选取二维的最大值所对应的索引，即概率分布中最大的值。使用tf.nn.softmax将预测结果处理到0 ~ 1之间，将loss_eyeglasses，loss_male，loss_smiling，loss_young相加来定义总的loss，接着定义正则化的loss，定义loss的代码如下：
```Python
loss_eyeglasses = tf.losses.sparse_softmax_cross_entropy(
labels= label_eyeglasses, logits = logits_eyeglasses)
loss_male = tf.losses.sparse_softmax_cross_entropy(
labels=label_male, logits = logits_male)
loss_smiling = tf.losses.sparse_softmax_cross_entropy(
labels = label_smiling, logits = logits_smiling)
loss_young = tf.losses.sparse_softmax_cross_entropy(
labels = label_young, logits = logits_young)
logits_eyeglasses = tf.nn.softmax(logits_eyeglasses)
logits_male = tf.nn.softmax(logits_male)
logits_smiling = tf.nn.softmax(logits_smiling)
logits_young = tf.nn.softmax(logits_young)
loss = loss_eyeglasses + loss_male + loss_smiling + loss_young
reg_set = tf.get_collection(tf.GraphKeys.REGULARIZATION_LOSSES)
l2_loss = tf.add_n(reg_set)
```
学习方式与学习率的定义：定义学习率的衰减方式为指数衰减，初始学习率为0.0001，衰减的步长为1000，每次衰减的比率为0.98，相关代码如下：

```Python
global_step = tf.Variable(0, trainable=True)
lr = tf.train.exponential_decay(0.0001, global_step,
                                decay_steps=1000,
                                decay_rate=0.98,
                                staircase=False)
```

优化器的定义;定义训练的方式为AdamOptimizer，传入学习率和loss，相关代码如下：

```Python
update_ops = tf.get_collection(tf.GraphKeys.UPDATE_OPS)
with tf.control_dependencies(update_ops):
    train_op = tf.train.AdamOptimizer(lr).minimize(loss + l2_loss, global_step)
```

对于训练集使用tf.train.shuffle_batch将队列中的数据打乱后再读取，在数据打包时，各个属性的label值用-1,1来表示，对这些值进行处理，将数据变为0,1，相关代码如下：

```Python
batch_im = features["image"]
    batch_eye = (features["Eyeglasses"] + 1) // 2
    batch_male = (features["Male"] + 1) // 2
    batch_smiling = (features["Smiling"] + 1) // 2
    batch_young = (features["Young"] + 1) // 2
    batch_im = tf.decode_raw(batch_im, tf.uint8)
```
模型共训练150000次，并对模型和日志信息进行保存，loss曲线如下图所示：

![Minion](/images/face/face12/6.jpg)

### 6.3 模型固化
模型前项推理基本与模型训练部分一致，需要注意的是inception_v3函数中的dropout需要修改为1.0，is_training修改为False。
为了使预测结果更直观，调用起来更方便，将softmax值转换成argmax值，得到概率分布值高的所对应的轴，实现代码如下：
```Python
logits_eyeglasses = tf.nn.softmax(logits_eyeglasses)
logits_male = tf.nn.softmax(logits_male)
logits_smiling = tf.nn.softmax(logits_smiling)
logits_young = tf.nn.softmax(logits_young)

logits_eyeglasses = tf.argmax(logits_eyeglasses, axis=1)
logits_male = tf.argmax(logits_male, axis=1)
logits_smiling = tf.argmax(logits_smiling, axis=1)
logits_young = tf.argmax(logits_young, axis=1)
```
使用convert_variables_to_constants函数将计算图中的变量取值以常量的形式保存。在人脸属性模型的多任务网络中，在模型固化的节点list中放入人脸属性的四个节点的tensor的name，从而转化多任务网络的pb模型，参数配置如下：
```Python
output_graph_def = tf.graph_util.\
        convert_variables_to_constants(session,
               session.graph.as_graph_def(),
               ['ArgMax',
               'ArgMax_1',
               'ArgMax_2',
               'ArgMax_3'])
```
### 6.4 模型测试
首先，读取打包好的pb文件，进行反序列化，恢复当前的graph。接着，获取需要推理的tensor，分别为pred_eyeglasses、pred_young、pred_male、pred_smiling，使用opencv读取测试图片，使用dlib库定义人脸检测器，提取出人脸框，并进行人脸数据的裁剪，计算当前四个tensor所对应的值，并在控制台打印输出结果，人脸属性模型测试结果如下图所示：

![Minion](/images/face/face12/7.png)
