# windows+hugo+github搭建个人博客


这篇文章介绍了如何在windows系统上基于github.io利用hugo搭建自己的个人博客。

<!--more-->

{{< admonition note "系统环境">}}  
* Windows 10  
* hugo_extended_0.68.3_Windows-64bit
{{< /admonition >}}

## 1 git

下载地址：<https://git-scm.com/downloads>
按步骤安装即可

## 2 hugo 

### 2.1 下载

下载地址：<https://github.com/gohugoio/hugo/releases>  
由于使用主题的限制，我使用的版本是:[hugo_extended_0.68.3_Windows-64bit](https://github.com/gohugoio/hugo/releases/download/v0.68.3/hugo_extended_0.68.3_Windows-64bit.zip)

### 2.2 配置

#### 创建文件夹

* `D:\hugo\bin`

#### 将解压后的hugo.exe放到新建的 D:\hugo\bin 目录下

#### 配置系统环境变量。

* 将 `D:\hugo\bin` 加到 `path` 中

#### 检查配置是否成功

进入`cmd`

```Bash
$ hugo version

显示下面的信息即表示安装成功
Hugo Static Site Generator v0.67.0-7F1DA3EF windows/386 BuildDate: 2020-03-09T20:37:49Z
```

### 2.3 生成站点
* 进入`D:\hugo\`下，单机鼠标右键选择 `Git Bash Here`

```Bash
$ hugo new site 文件名称（如 blog）
```

* 执行后，在hugo目录下会生成一个blog站点文件夹，目录结构如下：

├── archetypes    
├── content    
├── data    
├── layouts   
├── static   
├── themes  
├── config.toml  

{{< admonition note "笔记" >}}
* archetypes：存放default.md，头文件格式，每次新建文章默认显示的头部信息在此修改
* content：存放博客文章，markdown格式文件
* data：存放自定义或者导入的模板
* layouts：存放网站的数据模板
* static：存放图片、css、js等静态资源
* themes：存放主题文件，每个主题都是一个独立的文件夹
* config.toml：网站配置文件
{{< /admonition >}}

### 2.4 创建文章
* 进入**站点根目录**即`D:\hugo\blog`下，执行命令

```Bash
$ hugo new post/test.md
```
* 执行后，会自动在`content/post`下生成`test.md`文件，打开可编辑内容
* 文件头部的`draft`要改为`false`，这样部署后才能看到文章。
* 当前网站是没有任何内容的，需要下载个主题。

### 2.5 下载主题

* 主题下载地址：<https://themes.gohugo.io>
* 将下载好的主题解压后放到`themes`文件夹下
* 根据主题的说明进行相关配置（每个主题不一样，不做详细说明）
* 主题需要在`config.toml`中指定，如：theme = "LoveIt"
* 进入**站点根目录**即`D:\hugo\blog`下，执行命令启动服务

```Bash
$ hugo server

执行结果：
Building sites …
                   | EN  | ZH-CN
-------------------+-----+--------
  Pages            | 116 |   116
  Paginator pages  |   4 |     4
  Non-page files   |   0 |     0
  Static files     | 217 |   217
  Processed images |   0 |     0
  Aliases          |  35 |    34
  Sitemaps         |   2 |     1
  Cleaned          |   0 |     0

Built in 455 ms
Watching for changes in D:\hugo\blog\{archetypes,content,data,layouts,themes}
Watching for config changes in D:\hugo\blog\config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
* 访问<http://localhost:1313/>或<http://127.0.0.1:1313/>查看效果

## 3 部署到github pages

### 3.1 创建github.io仓库

* 登录github，点击左上角的`New`

![Minion](/images/github/github01/1.jpg)

* 创建 github.io 仓库，图中的 yourname 要与成自己的github用户名一致，即上图中 Owner 显示的用户名。
* 最后点击 Creat repository 完成创建

![Minion](/images/github/github01/2.jpg)

### 3.2 编译

* 将站点根目录`config.toml`中 baseURL 换成自己建立的仓库，如 `baseURL = "https://yourname.github.io/"`
* 进入站点根目录即`D:\hugo\blog`下，执行编译命令：

```Bash
$ hugo
```
* 执行后，站点根目录下会生成一个 public 文件夹，该文件下的内容即 Hugo 生成的整个静态网站。每次更新内容后，将 pubilc 目录里所有文件 push 到 GitHub 即可。

### 3.3 部署

进入`D:\hugo\blog\public`
首次使用执行以下命令：

```Bash
$ git init 

$ git remote add origin https://github.com/yourname/yourname.github.io.git 将本地目录链接到远程服务器的代码仓库

$ git add -A

$ git commit -m "first commit"

$ git push -u origin master
```

等待上传完成，就可以去 `yournamne.github.io` 看自己的博客了  
我的博客地址：<https://ieblyang.github.io/>，欢迎交流学习！

### 3.4 更新

以后每次**站点根目录**下执行`hugo`命令后，再到`public`下执行推送命令：

```Bash
$ git add -A

$ git commit -m "修改内容"

$ git push -u origin master  (会有弹窗提示，需要登录自己的github账号)

```

## 4 总结

整体来说并不复杂，最容易出问题的地方在“主题配置”部分，一定要按照主题作者的说明文档一步一步来，不是下载好放到`themes`文件夹下就算完成了，我使用的主题是 [LoveIt  :(far fa-heart):](https://github.com/dillonzq/LoveIt)，推荐你使用！


