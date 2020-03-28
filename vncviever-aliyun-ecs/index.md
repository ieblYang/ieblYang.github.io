# VNCViewer远程阿里云ECS服务器


这篇文章展示了通过VNCViewer远程控制阿里云ECS服务器的安装配置操作.

<!--more-->

{{< admonition note "系统环境">}}
* 桌面端版本：Windows 10  
* 远程端版本：Ubuntu 16.04  
* VNCViewer版本：6.20.113  
{{< /admonition >}}

## 1 安装配置好服务器

## 2 添加安全组

为`阿里云ECS`添加`安全组`

{{< admonition note "安全组">}}
* 协议类型：自定义TCP
* 端口范围：5900/59001
* 授权对象：0.0.0.0/0
{{< /admonition >}}

## 3 连接Terminal

通过自带的`Workbench`连接`Terminal`

## 4 安装VNCServer

### 4.1 更新系统软件

```Bash
sudo apt-get update 
```

### 4.2 安装vnc4server


```Bash
sudo apt-get install vnc4server
# 中间会有确认安装的提示，输入 Y 确认安装
```

### 4.3 启动vncserver

```Bash
vncserver
# 第一次启动会提示设置密码（19981112）
```

## 5 安装Ubuntu gnome界面

### 5.1 安装 x-windows 的基础

```Bash
sudo apt-get install x-window-system-core
```

### 5.2 安装登录管理器

```Bash
sudo apt-get install gdm
# 选择lightdm
```

### 5.3 安装Ubuntu界面程序

```Bash
sudo apt-get install ubuntu-desktop
```

### 5.4 安装Ubuntu界面其他依赖

```Bash
sudo apt-get install gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal
```

## 6 修改 `~/.vnc/xstartup\`

```Bash
vim ~/.vnc/xstartup  #用vim进行编辑
```

### 修改前

```Bash
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
x-window-manager &
```

### 修改后

```Bash
#!/bin/sh

# Uncomment the following two lines for normal desktop:
export XKL_XMODMAP_DISABLE=1
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
# x-window-manager &
gnome-session &
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
gnome-terminal &
```
## 7 启动vncserver

{{< admonition tip>}}
自动生成新桌面  
第一次生成:1  
第二次生成:2  
表示不同的桌面
{{< /admonition >}}

```Bash
vncviewer
```

## 8 关闭生成的桌面

```Bash
vncserver -kill :1      #:1表示桌面号
```

## 9 使用vnc工具连接

### 9.1 在官网下载VNCViewer

### 9.2 创建新的连接

{{< admonition note "创建新链接">}}
`File` -> `New connection`   
在`VNC Server`中输入**IP:桌面号**`39.105.95.4:1`
{{< /admonition >}}

## 10 参考资料

{{< admonition note "参考资料">}}
<https://www.jianshu.com/p/a4c99712a2b4>  
<https://blog.csdn.net/u012435142/article/details/82261586>
{{< /admonition >}}

