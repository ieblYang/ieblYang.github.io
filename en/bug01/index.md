# 解决ValueError: Object arrays ··· allow_pickle=False的报错


解决ValueError: Object arrays cannot be loaded when allow_pickle=False的报错

<!--more-->

### 系统配置

{{% admonition note "系统配置"%}}
* ubuntu 16.04
* python 3.5
* tensorflow 1.12.0
{{% /admonition %}}

### 报错信息

<<<<<<< HEAD
{{< admonition bug "报错信息">}}
=======
{{% admonition bug "报错信息"%}}
>>>>>>> add / update posts
ValueError: Object arrays cannot be loaded when allow_pickle=False
{{% /admonition %}}

### 问题分析

{{% admonition note "问题分析"%}}

```Bash
python3 src\align\align_dataset_mtcnn.py /root/py3_tensorflow/dataset/LFW /root/py3_tensorflow/dataset/LFW-160 --margin 32 --random_order --gpu_memory_fraction 0.25
```

* numpy版本过高
{{% /admonition %}}

### 修复方法

{{% admonition success "修复方法"%}} 
* 降低numpy版本为1.16.2

```Bash
sudo pip3 uninstall numpy
sudo pip3 install numpy==1.16.2
```

{{% /admonition %}}



