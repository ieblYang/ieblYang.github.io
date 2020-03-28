# 结合官方文档 入门Flask

结合Flask官方中文文档，学习Flask基础入门知识

<!--more-->

## 1 Flask介绍

### 1.1 简介

* Flask是一个基于Python实现的Web开发‘微’框架
	* 官方文档：<http://flask.pocoo.org/docs/0.12/>
	* 中文文档：<http://docs.jinkan.org/docs/flask/>

* Flask和Django一样，也是一个基于MVC设计模式的Web框架

* Flask依赖三个库
	* Jinja2 模板引擎
	* Werkzeug WSGI工具集
	* Itsdangerous基于Django的签名模块

### 1.2 流行原因

* 有非常齐全的官方文档，上手非常简单
* 有非常好的扩展机制和第三方扩展环境，工作中常见的软件都会有对应的扩展。自己动手实现扩展也很容易
* 社区活跃度非常高
* 微信框架的形式给开发者带来更大的选择空间

### 1.3 MVC(Model, View, Controller)设计模式

一种软件设计典范，用一种业务逻辑，使数据，界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面与用户交互的同时，不需要重新编写业务逻辑。

* MVC被独特的发展起来用于映射传统的输入，处理和输出功能在一个逻辑的图形化界面结构中。
* 核心思想：解耦
* 优点：降低各模块之间的耦合性，方便变更，更容易重构代码，最大程度实现了代码的重用。
* Model:用于封装与应用程序的业务逻辑相关的数据及对数据的处理方法，是Web应用程序中用于处理应用程序的数据逻辑部分，Model通常只提供功能性的接口，通过这些接口可以获取Model的所有功能。
* View:负责数据的显示和呈现，View是对用户 的直接输出。
* Controller:负贵从用户端收集用户的输入，可以看成提供View的反向功能，主要处理用户交互。

## 2 安装（Linux）

### 2.1 创建一个虚拟环境

创建一个项目文件夹，然后创建一个虚拟环境。创建完成项目文件夹中会有一个`venv`文件夹：

```Bash
mkdir myproject
cd myproject
python3 -m venv venv
```

### 2.2 激活虚拟环境

在开始工作前，先要激活响应的虚拟环境：

```Bash
.venv/bin/activate
```

### 2.3 安装Flask

在已激活的虚拟环境中可以使用如下命令安装Flask：

```Bash
pip3 install Flask
```

### 2.4 安装virtualenv

在Linux下，virtualenv 通过操作系统的包管理安装：

```Bash
# Debian,Ubuntu
sudo apt-get install python-virtualenv
```

## 3 Hello World

```Python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
	return 'Hello,World!'
	
if __name__ == '__main__':
	app.run()
```

{{< admonition note "笔记">}}
* 首先导入了`Flask`类。 该类的实例将会成为WSGI应用。
* 接着创建一个该类的实例。第一个参数是应用模块或者包的名称。如果使用一个单一模块(就像本例),
那么应当使用 `name` ，因为名称会根据这个模块是按应用方式使用还是作为一个模块导入而发生变化(可能
是 'main' ，也可能是实际导入的名称)。这个参数是必需的，这样 Flask 才能知道在哪里可以找到模板和
静态文件等东西。
* 然后使用 `route()` 装饰器来告诉 Flask 触发函数的 URL 。
* 函数名称被用于生成相关联的URL。函数最后返回需要在用户浏览器中显示的信息。
把它保存为 `hello. py` 或其他类似名称。不要使用`flask.py`作为应用名称，这会与 Flask 本身发生冲突。
{{< /admonition >}}

## 4 参数配置

### 4.1 利用app.run（）配置

* 在启动的时候还可以添加参数，在 `run()` 中
* debug 是否开启调试模式，开启后修改过python代码会自动重启
* threaded 是否开启多线程
* port 启动指定服务器的端口号
* host 主机，默认是127.0.0.1，指定为0.0.0.0代表本机所有ip

```Python
if __name__ == '__main__':
	app.run(debug=True,port=8000,host='0.0.0.0')
```

### 4.2 使用`config.py`配置文件

* 新建`config.py`

```Python
# 以DEBUG为例
DEBUG = True
```

* 载入配置文件

```Python
app.config.from_object('config')
```

* 配置文件读取

```Python
app.run(host='0.0.0.0', debug=app.config['DEBUG'], port=90)
```

{{< admonition abstract "重要">}}
* `app.config.from_object('config')` 载入配置文件，使用时`DEBUG`必须大写
* config 配置文件中的参数必须全部大写
* `DEBUG` 的值默认值为 False
{{< /admonition >}}

### 4.3 使用flask-script插件配置

#### 4.3.1 安装使用

* 安装

```Bash
pip3 install flask-script
```

* 使用app构建manager对象

```Python
manager = Manager(app=app)
```

* 使用manager启动程序

```Python
manager.run()
```

#### 4.3.2 参数使用

```
-d		#是否开启调试模式
-r 		#是否自动重新加载文件
-h, -host #指定主机
-p, -port	#指定端口
-threaded	#是否使用多线程
-?, -help	#查看帮助
#示例：
python3 manage.py runserver --help
```

## 5 请求流程

![Minion](/images/flask/flask01/1.jpg)

## 6 路由

路由：将从客户端发送过来的请求分发到指定函数上

```Python
@app.route('/')
def index():
	return 'Index Page'

@app.route('/hello')
def hello():
	return 'Hello,World'
```

### 6.1 变量规则

```Python
@app.route('/user/<username>')
def show_user_profile(username):
	# show the user profile for that user
	return 'User %s' % escape(username)

@app.route('/post/<int:post_id>')
def show_post(post_id):
	# show the post with the given id, the id is an integer
	return 'Post %d' % post_id

@app.route('/path/<path:subpath>')
def show_subpath(subpath):
	# show the subpath after /path/
	return 'Subpath %s' % escape(subpath)
```

{{< admonition note "笔记">}}
参数
* 路径参数
	* 位置参数
	* 关键字参数
* 请求参数
	* get 参数在路径中？之后
	* post参数在请求体中
Flask中参数
* 都是关键字参数
* 默认标识是`<>`
* name需要和对应的视图函数的参数名字保持一致
* 参数允许有默认值
	* 如果有默认值，那么在路由中，不传输参数也可以
	* 如果没有默认值，参数在路由中必须传递
* 默认参数类型是字符串,写法 `<converter:variable_name>`
* converter类型
	* string 默认值，会将`/`认为是参数分割符
	* int 接收整型
	* float 接收浮点型
	* path 接收到的数据格式是字符串，会将斜线`/`认为是一个字符
	* uuid 只接收uuid字符串，唯一码，一种生成规则
	* any 列出的元组中的任意一个
{{< /admonition >}}

### 6.2 唯一的URL / 重定向行为

以下的两条规则的不同之处在于是否使用尾部的`/`:
```Python
@app.route('/projects/')
def projects():
	return 'The project page'

@app.route('/about')
def about():
	return 'The about page'
```

{{< admonition note "笔记">}}
* `projects` 的 URL 是中规中矩的，尾部有一个斜杠，看起来就如同一个文件夹。 访问一个没有斜杠结尾的 URL 时 Flask 会自动进行重定向，在尾部加上一个斜杠。

* `about` 的 URL 没有尾部斜杠，因此其行为表现与一个文件类似。如果访问这个 URL 时添加了尾部斜杠就会得到一个 404 错误。这样可以保持 URL 唯一，并帮助 搜索引擎避免重复索引同一页面。
{{< /admonition >}}

### 6.3 反向解析

`url_for('函数名',参数名=value)` 

* 根据endpoint获取到对应的路径
* endpoint默认就是函数的名字
* 如果有参数 url_for('函数名',key=value,key=value)
* 反向解析在模板上可以直接使用

* 使用在app中
	* url_for('endpoint')
	* endpoint 默认是函数名字

* 使用在blueprint中
	* url_for('bluename.endpoint')
	* 蓝图名字.函数名

* 获取静态资源路径
	* url_for('static',filename='path')
	* static 资源
	* path 相对于资源的路径 

{{< admonition note "笔记">}}
为什么不在把 URL 写死在模板中，而要使用反转函数 `url_for()` 动态构建？
* 反转通常比硬编码 URL 的描述性更好。
* 可以只在一个地方改变 URL ，而不用到处乱找。
* URL 创建会为你处理特殊字符的转义和 Unicode 数据，比较直观。
* 生产的路径总是绝对路径，可以避免相对路径产生副作用。
* 如果应用是放在 URL 根路径之外的地方（如在 `/myapplication` 中，不在 `/` 中）， `url_for()` 会妥善处理。
{{< /admonition >}}

```Python
from flask import Flask, escape, url_for

app = Flask(__name__)

@app.route('/')
def index():
    return 'index'

@app.route('/login')
def login():
    return 'login'

@app.route('/user/<username>')
def profile(username):
    return '{}\'s profile'.format(escape(username))

with app.test_request_context():
    print(url_for('index'))
    print(url_for('login'))
    print(url_for('login', next='/'))
    print(url_for('profile', username='John Doe'))
#运行结果
/  
/login  
/login?next=/  
/user/John%20Doe  
```

### 6.4 HTTP 方法

Web 应用使用不同的 HTTP 方法处理 URL 。当你使用 Flask 时，应当熟悉 HTTP 方法。 缺省情况下，一个路由只回应 `GET` 请求。 可以使用 `route()` 装饰器的 `methods` 参数来处理不同的 HTTP 方法:

```Python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()

```

methods中指定请求方法

* GET
* POST
* HEAD
* PUT
* DELETE

## 7 静态文件

动态的 web 应用也需要静态文件，一般是 CSS 和 JavaScript 理想情况下服务器已经配置好了提供静态文件的服务。但是在开发过程中， Flask 也能做好这项工作。只要在包或模块旁边创建一个名为 `static` 的文件夹就行了。 静态文件位于应用的 `/static` 中。  
使用特定的 'static' 端点就可以生成相应的 URL

```Python
url_for('static', filename='style.css')
# 这个静态文件在文件系统中的位置应该是 static/style.css
```

## 8 模板

模板是呈现给用户的界面，在MVT中充当T的角色，实现了VT的解耦，开发中VT有着N:M的关系，一个V可以通用任意T，一个T可以被任意V调用。

* 模板处理分为两个过程：
	* 加载
	* 渲染

* 模板代码包含两个部分：
	* 静态HTML
	* 动态插入的代码段

Flask 会在 `templates` 文件夹内寻找模板。因此，如果应用是一个模块， 那么模板文件夹应该在模块旁边；如果是一个包，那么就应该在包里面：

* 情形 1 : 一个模块:

```
/application.py
/templates
    /hello.html
```

* 情形 2 : 一个包:

```
/application
    /__init__.py
    /templates
        /hello.html
```

### 8.1 Jinja2

Flask中使用Jinja2模板引擎
Jinja2由Flask作者开发
* 一个现代化设计和友好的Python模板语言
* 模仿Django的模板引擎

* 优点：
	* 速度快，被广泛使用
	* HTML设计和后端Python分离
	* 较少Python复杂度
	* 非常灵活，快速和安全
	* 提供了控制，继承等高级功能

* Flask 自动配置 Jinja2 模板引擎。

使用 `render_template()`方法可以渲染模板，只要提供模板名称和需要作为参数传递给模板的变量就行了。下面是一个简单的模板渲染例子:

```Python
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```

### 8.2 模板语法

模板语法主要分为两种：变量、标签

#### 8.2.1 变量

模板中的变量`{{ var }}`
* 视图传递给模板的数据
* 前面定义出来的数据
* 变量不存在，默认忽略

#### 8.2.2 标签

模板中的标签`{% tag %}`
* 逻辑控制
* 使用外部表达式
* 创建变量
* 宏定义

### 8.3 结构标签

#### 8.3.1 block

```Html
{% block xxx %}
{% endblock %}
```

* 块操作
* 父模板挖坑，子模板填坑
* 首次出现挖坑，非首次填坑
* 多次填坑会出现覆盖，不想覆盖使用{{super()}}

#### 8.3.2 extends

```Html
{% extends 'xxx' %}
<!--继承后保留块中的内容-->
{{super()}}
```

#### 8.3.3 include

```Html
{% include 'xxx' %}
```

* 包含，将其他html包含进来，体现的是由零到一的概念
* 能用block + extends实现的，尽量不要使用include

#### 8.3.4 marco

```Html
{% marco hello(name) %}	
{% endmarco %}
{{name}}
```

* 宏定义，可以在html中定义函数
* 可以接收参数
* 通过调用函数生成html
* 支持导入操作

```Html
{% from xxx import yy %}
```

### 8.4 for 循环

```Html
{% for item in cols %}
	AA
{% else %}
	BB
{% endfor %}
```

* 可以使用和Python一样的`for...else`
* 也可以获取循环信息loop
	* loop.first
	* loop.last
	* loop.index
	* loop.index()
	* loop.revindex
	* loop.revindex()

### 8.5 过滤器

```Html
{{变量|过滤器|过滤器...}}
```

* 过滤器并不是先写先执行  
* safe最后做  

|          |       |
| -------- | ----- |
|capitalize|驼峰命名|
|lower	   |变为小写|
|upper	   |变为大写|
|title 	   |标题|
|trim	   |去掉空格|
|reverse   |反转|
|format	   |格式化|
|striptags |渲染之前，将值中标签去掉|

### 8.6 Flask-Bootstrap

官方文档：[Flask-Bootstrap](https://pythonhosted.org/Flask-Bootstrap/chinese_version.html)

#### 8.6.1 安装

```Bsah
pip3 install flask-bootstrap
```

#### 8.6.2 导入和加载扩展

```Python
from flask import Flask
from flask_bootstrap import Bootstrap

def create_app():
  app = Flask(__name__)
  Bootstrap(app)

  return app

# do something with app...
```

#### 8.6.2 创建基于Bootstrap的模板

```Html
{% extends "bootstrap/base.html" %}
{% block title %}This is an example page{% endblock %}

{% block navbar %}
<div class="navbar navbar-fixed-top">
  <!-- ... -->
</div>
{% endblock %}

{% block content %}
  <h1>Hello, Bootstrap</h1>
{% endblock %}
```

## 9 Request

```Python
from flask import request

@app.route('/request', methods=['GET', 'POST'])
def req():
	print(request)
	print(request.method)
	print(request.data)
	# arguments 参数，get请求参数
	print(request.args)
	print(request.args.get('name'))
	# post 相关请求都会有数据
	print(request.form)
	print(request.files)
	print(requset.cookies)
	#ip地址
	print(requset.remote_addr)
	# 浏览器标识
	print(requset.user_agent)

	print(requset.user_url)

```

## 10 Response

服务器返回给客户端的数据
由程序员创建，返回Response对象  
* 直接返回Response对象

* 通过make_response(data,code)
	* -data 返回的数据内容
	* -code 状态码

* 返回文本的内容，状态码
* 返回模板  

```Python
@app.route('/response')
def(resp):
	
	result = render_template('hello.html')
	
	print(result)
	
	print(type(result))

	response = make_response('<h3>响应</h3>',400)

	print(response)

	print(type(response))

	return response

```

## 11 重定向和错误

* redirect()
* url_for('函数名',参数=value)
* 使用 `redirect()` 函数可以重定向。使用 `abort()` 可以更早退出请求，并返回错误代码:

```Python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()

# 上例实际上是没有意义的，它让一个用户从索引页重定向到一个无法访问的页面（401 表示禁止访问）。
# 但是上例可以说明重定向和出错跳出是如何工作的。
``` 

* 缺省情况下每种出错代码都会对应显示一个黑白的出错页面。使用 errorhandler() 装饰器可以定制出错页面:

```Python
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
#注意 render_template() 后面的 404 ，这表示页面对应的出错 代码是 404 ，即页面不存在。缺省情况下 200 表示：一切正常。
```

## 12 JSON

JSON 格式的响应是常见的，用 Flask 写这样的 API 是很容易上手的。如果从视图返回一个 `dict` ，那么它会被转换为一个 JSON 响应。

```Python
@app.route("/me")
def me_api():
    user = get_current_user()
    return {
        "username": user.username,
        "theme": user.theme,
        "image": url_for("user_image", filename=user.image),
    }
```

如果 `dict` 还不能满足需求，还需要创建其他类型的 JSON 格式响应，可以使用 `jsonify()` 函数。该函数会序列化任何支持的 JSON 数据类型。 

```Python
@app.route("/users")
def users_api():
    users = get_all_users()
    return jsonify([user.to_json() for user in users])
```

## 13 蓝图

* Blueprint 是一种组织一组相关视图及其他代码的方式。与把视图及其他 代码直接注册到应用的方式不同，蓝图方式是把它们注册到蓝图，然后在工厂函数中把蓝图注册到应用。  
* flask中用来解决上帝文件问题，将请求从主文件中拆分到多个文件中。

### 13.1 插件安装

```Bash
pip3 install flask-blueprint
```

### 13.2 初始化蓝图

```Python
# manager.py
from flask import Flask
from flask_script import Manager

from App.views import bp

app = Flask(__name__)

# 在app中注册
app.register_blueprint(blueprint=bp)

manager = Manager(app=app)

if __name__ == '__main__'
	manager.run()

# /App/views
from flask import Blueprint

# 创建一个类，构造一个蓝图
bp = Blueprint('bp',__name__)

# 使用
@bp.route('/')
def.hello():
	return 'Hello Blueprint!'
```

## 14 Cookie

* 客户端会话技术
* 数据都是存储在浏览器中
* 支持过期
* 不能跨域名
* 不能跨浏览器
* cookie是通过Response来进行操作

* flask中的cookie可以直接支持中文
	* flask对cookie中的内容做了编码

```Python
# 读取cookie
from flask import request

@app.route('/')
def index():
    username = request.cookies.get('username')
    # use cookies.get(key) instead of cookies[key] to not get a
    # KeyError if the cookie is missing.

# 存储cookie
from flask import make_response

@app.route('/')
def index():
    resp = make_response(render_template(...))
    resp.set_cookie('username', 'the username')
    return resp
```

## 15 Session

* 服务端会话技术
* 对数据进行数据安全操作

* 默认在内存中
	* 不容易管理
	* 容易丢失
	* 不能多台电脑协作

* Flask-Session 默认有效期31天

### 15.1 Session使用

```Python
from flask import Flask, session, redirect, url_for, escape, request

app = Flask(__name__)
# 使用会话之前必须设置一个密钥。
# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return 'Logged in as %s' % escape(session['username'])
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))
```
### 15.2 Flask-Session插件

官方文档：[Flask-Session](https://pythonhosted.org/Flask-Session/)

* 安装

```Bash
pip3 install Flask-Session
```

* 使用

```Python
from flask import Flask, session
from flask.ext.session import Session

app = Flask(__name__)
# Check Configuration section for more details

app.config['SESSION_TYPE'] = 'redis'
Session(app=app)

@app.route('/set/')
def set():
    session['key'] = 'value'
    return 'ok'

@app.route('/get/')
def get():
    return session.get('key', 'not set')
```

## 16 消息闪现

* 闪现系统的基本工作原理是在请求结束时记录一个消息，提供且只提供给下一个请求使用。通常通过一个布局模板来展现闪现的消息。  
* `flash()` 用于闪现一个消息。在模板中，使用 get_flashed_messages() 来操作消息。

## 17 登录示例

```Python
from flask import Flask, flash,session, redirect, render_template, url_for, escape, request

app = Flask(__name__)

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return 'Logged in as %s' % escape(session['username'])
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
    	 if request.form['username'] != 'admin' or \
           request.form['password'] != 'secret':
            flash(u'Invalid password provided', 'error')
        else:
            flash('You were successfully logged in')
            return redirect(url_for('index'))
       
    return  render_template('login.html', error=error)

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))
```

以下是实现闪现的 `layout.html` 模板：

```Html
<!doctype html>
<title>My Application</title>
{% with messages = get_flashed_messages() %}
  {% if messages %}
    <ul class=flashes>
    {% for category, message in messages %}
      <li> class="{{ category }}">{{ message }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
{% block body %}{% endblock %}
```

以下是继承自 `layout.html` 的 `index.html` 模板：

```Html
{% block body %}
  <h1>Overview</h1>
  <p>Do you want to <a href="{{ url_for('login') }}">log in?</a>
{% endblock %}
```

以下是同样继承自 `layout.html` 的 `login.html` 模板：

```Html
{% extends "layout.html" %}
{% block body %}
  <h1>Login</h1>
  {% if error %}
    <p class=error><strong>Error:</strong> {{ error }}
  {% endif %}
  <form method=post>
    <dl>
      <dt>Username:
      <dd><input type=text name=username value="{{
          request.form.username }}">
      <dt>Password:
      <dd><input type=password name=password>
    </dl>
    <p><input type=submit value=Login>
  </form>
{% endblock %}
```

