# Ubuntu16.04安装pycharm


这篇文章展示了如何在Ubuntu16.04系统中安装Pycharm并进行基本配置.
<!--more-->

{{< admonition note "系统环境">}}  
* Ubuntu 16.04  
{{< /admonition >}}

## 1 下载

* 在pycharm官网下载所需版本:  
<https://www.jetbrains.com/pycharm/download/#section=linux> 

* 建议下载专业版  

* 将下载得到的安装包移动到Home文件夹下

## 2 解压

### 2.1打开终端，输入解压指令

```Bash
tar -zxvf pycharm-2019.3.3.tar.gz
```

### 2.2 得到解压后的文件`pycharm-2019.3.3`

## 3 启动

### 3.1 运行`pycharm.sh`

```Bash
# 进入 pycharm-2019.3.3/bin 目录下 运行pycharm.sh
cd pycharm-2019.3.3/bin
sh pycharm.sh
```

### 3.2 进行激活等配置后进入软件

## 4 配置

### 4.1 调出pycharm顶部菜单栏

{{< admonition note "配置">}}  
* 按`Ctrl + Shift + A`打开`Find Action`对话框，键入`Experimental features`，然后按Enter键。
* 取消`linux.native.menu`选项旁边的复选框，应用更改并关闭对话框。
* 重启PyCharm
{{< /admonition >}}

### 4.2 将pycharm固定到系统桌面菜单栏

{{< admonition note "配置">}} 
顶部菜单栏->`tools`->`Create Desktop Entry…`
{{< /admonition >}}

## 5 完成

基本配置结束，下次直接双击桌面图标即可启动！

## 6 参考资料

{{< admonition note "参考资料">}}
https://blog.csdn.net/shuiyixin/article/details/89530415
https://blog.csdn.net/loco1223/article/details/91127366
https://blog.csdn.net/Boogyman/article/details/100527532
https://blog.csdn.net/qq_42517220/article/details/86756538
{{< /admonition >}}

