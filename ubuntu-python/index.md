# Ubuntu16.04安装Python3.5


这篇文章展示了如何在Ubuntu16.04系统中安装Python3.5并将python3.5设置为默认版本.

<!--more-->

{{< admonition note "系统环境">}}  
* Ubuntu 16.04  
* 原始python版本 python2.7
* 安装python版本 python3.5
{{< /admonition >}}

## 1 ubuntu安装Python3.5

```Bash
sudo add-apt-repository ppa:fkrull/deadsnakes
sudo apt-get update              #检查系统更新
sudo apt-get install python3.5
python --version                 #查看当前版本号
sudo apt-get install python3-pip #装pip3
```

## 2 设置python3为默认版本

### 2.1 执行命令

```Bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150
```

### 2.2 如果要切换回 python2

```Bash
sudo update-alternatives --config python
#按照提示输入选择数字回车即可
```

## 3 参考资料

{{< admonition note "参考资料">}}
https://blog.csdn.net/qq_36801146/article/details/89380491
{{< /admonition >}}

