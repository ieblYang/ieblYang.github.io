# 解决 ModuleNotFoundError: No module named 'flask._compat'的报错


解决 ModuleNotFoundError: No module named 'flask._compat' 的报错

<!--more-->

### 系统配置

{{% admonition note "系统配置"%}}
* windows 11
* python 3.8
* Pycharm 2021.3.1
{{% /admonition %}}

### 报错信息
{{% admonition bug "报错信息"%}}
ModuleNotFoundError: No module named 'flask._compat'
{{% /admonition %}}

### 问题分析

{{% admonition note "问题分析"%}}

* 系统默认下载了最新版本的 Flask
* Flask扩展升级到 2.0 后删除了一些文件，导致出现问题

{{% /admonition %}}

### 修复方法

{{% admonition success "修复方法"%}}
* 降低Flask的版本为1.1.4

```Bash
pip uninstall flask
pip install Flask==1.1.4
```
{{% /admonition %}}



