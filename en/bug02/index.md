# 解决AttributeError: module 'scipy.misc' ···  'imread'的报错


解决AttributeError: module 'scipy.misc' has no attribute 'imread'的报错

<!--more-->

### 系统配置

{{% admonition note "系统配置"%}}
* ubuntu 16.04
* python 3.5
* tensorflow 1.12.0
{{% /admonition %}}

### 报错信息
{{% admonition bug "报错信息"%}}
module 'scipy_misc' has no attribute 'imread'
{{% /admonition %}}

### 问题分析

{{% admonition note "问题分析"%}}

```Bash
python3 src\align\align_dataset_mtcnn.py /root/py3_tensorflow/dataset/LFW /root/py3_tensorflow/dataset/LFW-160 --margin 32 --random_order --gpu_memory_fraction 0.25
```

* 在解析LFW数据集时报错，应该是在安装FaceNet的依赖的时候没有在`requirements.txt`中限定版本，默认安装了高版本的scipy
* imread is deprecated! imread is deprecated in SciPy 1.0.0, and will be removed in 1.2.0. Use imageio.imread instead.
{{% /admonition %}}

### 修复方法

{{% admonition success "修复方法"%}}
* 降低scipy的版本为1.2.1

```Bash
sudo pip3 uninstall scipy
sudo pip3 install scipy==1.2.1
```
{{% /admonition %}}



