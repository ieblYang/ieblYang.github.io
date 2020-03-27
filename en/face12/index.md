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

{{< admonition note "笔记">}}
* `identity_CelebA.txt`	身份标记：图片名 + 编号，相同编号代表同一人，如：000001.jpg 2880
* `list_attr_celeba.txt`  标记图片属性  
* `list_landmarks_align_celeba.txt`  对齐后的图片，人脸标记（眼鼻嘴）`lefteye_x` `lefteye_y` `righteye_x` `righteye_y` `nose_x` `nose_y` `leftmouth_x` `leftmouth_y` `rightmouth_x` `rightmouth_y`  
* `list_landmarks_celeba.txt` 自然环境下的图片，人脸标记（眼鼻嘴）`lefteye_x` `lefteye_y` `righteye_x` `righteye_y` `nose_x` `nose_y` `leftmouth_x` `leftmouth_y` `rightmouth_x` `rightmouth_y`  
* `list_eval_partition.txt`  分组：训练、验证、测试  
{{< /admonition >}}

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

>
>数据处理代码后期上传！
>
> -- _ieblYang_

