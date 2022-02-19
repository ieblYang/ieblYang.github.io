# Ubuntu16.04安装Mysql数据库


这篇文章展示了如何在Ubuntu16.04系统中安装Mysql数据库，并安装可视化工具WorkBench

<!--more-->

{{% admonition note "系统环境"%}}  
* Ubuntu 16.04  
{{% /admonition %}}

## 1 ubuntu安装Mysql

```Bash
# 更新安装源
sudo apt-get update
# 安装mysql
sudo apt-get install mysql-server
# 安装过程中按提示设置root用户的密码
```

{{% admonition note "笔记"%}}
上述命令会安装以下包：
* apparmor
* mysql-client-5.7
* mysql-common
* mysql-server
* mysql-server-5.7
* mysql-server-core-5.7
{{% /admonition %}}

## 2 启动和关闭mysql服务器

```Bash
# 启动服务器
service mysql start
# 关闭服务器
service mysql stop

# 确认是否启动成功，若mysql节点处于LiSTEN状态表示启动成功
sudo netstat -tap | grep mysql

```

## 3 进入mysql shell界面

```Bash
mysql -u root -p
```

## 4 安装可视化工具mysql-workbench

```Bash
sudo apt-get install mysql-workbench
```
## 5 参考资料
{{% admonition note "笔记"%}}
https://blog.csdn.net/weixin_42209572/article/details/98983741
https://blog.csdn.net/bianchengxiaosheng/article/details/78494648
{{% /admonition %}}


