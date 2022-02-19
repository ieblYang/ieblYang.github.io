# 解决 ModuleNotFoundError: No module named 'MySQLdb' 的报错


解决 ModuleNotFoundError: No module named 'MySQLdb' 的报错

<!--more-->

### 系统配置

{{% admonition note "系统配置"%}}
* windows 11
* python 3.8
* Pycharm 2021.3.1
{{% /admonition %}}

### 报错信息
{{% admonition bug "报错信息"%}}

ModuleNotFoundError: No module named 'MySQLdb'

{{% /admonition %}}

### 问题分析

{{% admonition note "问题分析"%}}

* 因为 `SQLAlchemy` 连接 `Mysql` 默认驱动是 `MySQLdb` ，`MySQLdb` 仅支持 `Python2.x` ,不支持 `Python3` 的版本
* 如果使用 `Python3.x` 版本时，需要安装并使用额外的库 `pymysql`

{{% /admonition %}}

### 修复方法

{{% admonition success "修复方法"%}}
* 安装 pymysql 
```Bash
pip install pymysql
```

* 引入 pymysql 包
```Python
import pymysql
pymysql.install_as_MySQLdb()
```

* 将 `mysql://user:YourPassword@1127.0.0.1:3306/database` 改为 ` mysql+pymysql://user:YourPassword@127.0.0.1:3306/database`
{{% /admonition %}}



