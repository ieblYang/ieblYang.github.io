# Ubuntu16.04安装桌面后只能访客登录


这篇文章展示了如何解决ubuntu16.04安装桌面后只能访客登录的方法.

<!--more-->

{{< admonition note "系统环境">}}  
* Ubuntu 16.04  
* Aliyun Workbench
* Aliyun VNC
{{< /admonition >}}

{{< admonition warning>}}  
* 图形化桌面安装好后，远程连接进去，发现只能使用guest帐号，不能选择其他用户
* 登录进去有警告信息！
{{< /admonition >}}

## 1 连接Terminal

通过云服务器管理控制台的**Workbench**连接Terminal

## 2 修改root权限

修改`/usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf`文件

```Bash
vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
```

### 修改前

```Bash
[Seat:*]
user-session=ubuntu
```

### 修改后

```Bash
[Seat:*]
user-session=ubuntu
greeter-show-manual-login=true
allow-guest=false
```

## 3 重启服务器

```Bash
reboot
```

## 4 关闭警告信息

重启之后就可以用root用户登录，登录后还是有**警告** 

### 修改`/root/.profile`文件

#### 修改前

```Bash
   # ~/.profile: executed by Bourne-compatible login shells.
 
    if [ "$BASH" ]; then
      if [ -f ~/.bashrc ]; then
        . ~/.bashrc
      fi
    fi
    mesg n || true
 
```

#### 修改后

```Bash
   # ~/.profile: executed by Bourne-compatible login shells.
 
    if [ "$BASH" ]; then
      if [ -f ~/.bashrc ]; then
        . ~/.bashrc
      fi
    fi
    tty -s && mesg n || true
```

## 5 重启服务器

```Bash
reboot
```

## 6 完成

重启之后，只有root用户，登录后也没有警告信息

## 7 参考资料

{{< admonition note "参考资料">}}
https://blog.csdn.net/Never_Give_up_z/article/details/83190285
{{< /admonition >}}

