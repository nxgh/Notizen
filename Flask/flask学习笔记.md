
## Hi Flask


## Chapter 0 Ready

### 1. Git

令查看当前项目仓库中包含的所有标 签：
`git tag -n`

使用git checkout命令即可签出对应标签版本的代码，添加标签名作为参数
`git checkout <tag-name>`

如果在执行新的签出命令之前，你对文件做了修改，那么需要使用 git reset命令来撤销改动：
`git reset`
git reset命令会删除本地修改

如果你想比较两个版本之间的变化，可以使用git diff命令
`git diff <tag-one> <tag-two>`



### 2. 环境搭建

虚拟环境切换
**pyenv** 

版本环境切换
**pipenv**

Pipenv会自动帮你管理虚拟环境和依赖文件，并且提供了一系列命令和选项来帮助你实现各种依赖和环境管理相关的操作。简而言之，它更方便、完善和安全。

**安装pipenv**

`pip install pipenv`

**创建虚拟环境**

`pipenv install`

**激活虚拟环境**

`pipenv shell`

不显式激活虚拟环境即可在当前项目的虚拟环境中执行命 令

`pipenv run python hello.py`


**安装依赖到虚拟环境**

`pipenv install flask`

**退出虚拟环境**

`exit`

**记录依赖**

Pipenv会在你创建虚拟环境时自动创建Pipfile和Pipfile.lock文件(如果不存在)，并且会在你使用pipenv install和pipenv uninstall命令安装和卸载包时自动更新Pipfile和Pipfile.lock。

**在部署环境安装依赖**

执行pipenv install，它会自动安装Pipfile中记录的依赖：

`pipenv install`

**区分开发依赖**

使用Pipenv时，你只需要在安装依赖时添加一个--dev选项，它会自动被分类为开发依赖(写入Pipfile的dev-packages一节中)

`pipenv install pytest --dev`

在新的开发环境安装依赖时 在pipenv install命令后添加--dev选项即可一并安装开发依赖：

`pipenv install --dev`

**更换Pypi源(阿里云)**

```
[[source]]

url = "https://mirrors.aliyun.com/pypi/simple"
verify_ssl = true
name = "pypi"
```

也可以在执行安装命令时通过--pypi-mirror选项指定PyPI源，比如：

`$ pipenv install --pypi-mirror https://mirrors.aliyun.com/pypi/simple
`

**自定义虚拟环境文件夹路径**

默认情况下，Pipenv会自动为你选择虚拟环境的存储位置
在Windows下通常为`C:\Users\Administrator\.virtualenvs\，`
而Linux或macOS则为`~/.local/share/virtualenvs/`

如果你想将虚拟环境文件夹在项目目录内创建，可以设置环境变量`PIPENV_VENV_IN_PROJECT`，这时名为.venv的虚拟环境文件夹将在项目根目录被创建。另外你也可以通过`WORKON_HOME`环境变量来自定义存储路径。


**Flask 的依赖包**

|              |                  |
| :----------- | :--------------- |
| Jinja2       | 模板渲染引擎     |
| MarkupSafe   | HTML字符转义工具 |
| Werkzeug     | WSGI 工具集      |
| click        | 命令行工具       |
| itsdangerous | 加密签名功能     |

**WSGI**

WSGI(Web Server Gateway Interface)是Python中用来规定Web服务器如何与Python Web程序进行沟通的标准, Werkzeug是Flask 的WSGI 工具集,处理请求与相应,内置WSGI开发服务器,调试器和重载器


## Chapter 1 Hello Flask

### 最小的 Flask 程序

```py
# app.py
from flask import Flask
# 先从flask包导 入Flask类，这个类表示一个Flask程序
app = Flask(__name__)
# 实例化这个类，就得到我们的 程序实例app

@app.route('/')
def index():
    return '<h1>Hello Flask</h1>'
```
传入Flask类构造方法的第一个参数是模块或包的名称，我们应该使用特殊变量`__name__`。Python会根据所处的模块来赋予`__name__`变量相 应的值，对于我们的程序来说(app.py)，这个值为app。除此之外，这也会帮助Flask在相应的文件夹里找到需要的资源，比如模板和静态文件。
 

### 注册路由

在一个Web应用里，客户端和服务器上的Flask程序的交互可以简单概括为以下几步：
1. 用户在浏览器输入URL访问某个资源。
2. Flask接收用户请求并分析请求的URL。
3. 为这个URL找到对应的处理函数。
4. 执行函数并生成响应，返回给浏览器。
5. 浏览器接收并解析响应，将信息显示在页面中。

我们要做的只是建立处理请求的函数，并为其定义对应的URL规则。

```py

@app.route('/')
def index():
    pass
```
注册路由: 为函数附加app.route() 装饰器,并传入URL规则作为参数,就可以让URL与函数建立关联

路由负责管理URL和函数之间的映射，而这个函数则被称为视图函数(view function)。
 
路由: 作为动词时，它的含义是“按某路线发送”，即调用与请求URL对应的视图函数。

app.route()装饰器把根地址`/`和index()函数绑定起来，当用户访问这个URL时就会触发index()函数。这个视图函数可以像其他普通函数一样执行任意操作，比如从数据库中获取信息，获取请求信息，对用户输入的数据进行计算和处理等。最后，视图函数返回的值将作为响应的主体

### 启动开发服务器

Flask内置了一个简单的开发服务器(由依赖包Werkzeug提供)，足够在开发和测试阶段使用，在生产环境需要使用性能够好的生产服务器，以提升安全和性能

Flask通过依赖包Click内置了一个CLI(Command Line Interface，命令行交互界面)系统。当我们安装Flask后，会自动添加一个flask命令脚本，我们可以通过flask命令执行内置命令、扩展提供的命令或是我们自己定义的命令。其中，`flask run`命令用来启动内置的开发服务器

`flask run`

1. 自动发现程序实例

为Flask会自动 探测程序实例，自动探测存在下面这些规则：
 - 从当前目录寻找app.py和wsgi.py模块，并从中寻找名为app或 application的程序实例。
 - 从环境变量FLASK_APP对应的值寻找名为app或application的程序实例。


如果你的程序主模块是其他名称，比如hello.py， 那么需要设置环境变量FLASK_APP，将包含程序实例的模块名赋值给这个变量。
Linux或macOS系统使用export命令：

`$ export FLASK_APP=hello`

在Windows系统中使用set命令：

`> set FLASK_APP=hello`

2. 管理环境变量

如果安装了python-dotenv，那么在使用flask run或其他命令时会使用它自动从`.flaskenv`文件和`.env`文件中加载环境变量。
 
当安装了python-dotenv时，Flask在加载环境变量的优先级是：
手动设置的环境变量>.env中设置的环境变量>.flaskenv设置的环境变量。

**安装python-dotenv**

`pipenv install python-dotenv`

我们在项目根目录下分别创建两个文件：.env和.flaskenv。
`.flaskenv`用来存储和Flask相关的公开环境变量，比如FLASK_APP；
`.env`用来存储包含敏感信息的环境变量，比如用来配置Email服务器的账户名与密码。

```conf
# .flasken
# 以 # 开头的是注释
FLASK_APP=hello.py
FLASK_RUN_HOST=192.168.1.1
FLAKS_RUN_PORT=8080
FLASK_ENV=development
```
Flask提供了一个FLASK_ENV环境变量用来设置环境，默认为production(生产)。在开发时，我们可以将其设为development(开发)，这会开启所有支持开发的特性。在开发环境下，调试模式(Debug Mode)将被开启，这时执行flask run启动程序会自动激活Werkzeug内置的调试器(debugger)和重载器 (reloader)，它们会为开发带来很大的帮助。
 
1. 调试器
调试器允许你在错误页面上执行Python代码。单击错误信息右侧的命令行图标，会弹出窗口要求输入PIN码，也就是在启动服务器时命令行窗口打印出的调试器PIN码（DebuggerPIN）。输入PIN码后，我们可以单击错误堆栈的某个节点右侧的命令行界面图标，这会打开一个包含代码执行上下文信息的PythonShell，我们可以利用它来进行调试。

2.重载器
当我们对代码做了修改后，期望的行为是这些改动立刻作用到程序上。重载器的作用就是监测文件变动，然后重新启动开发服务器。

默认会使用Werkzeug内置的stat重载器，它的缺点是耗电较严重，而且准确性一般。。为了获得更优秀的体验，我们可以安装另一个用于监测文件变动的Python库Watchdog

`pipenv install watchdog --dev`

### Python shell

在开发Flask程序时，我们并不会直接使用python命令启动Python Shell，而是使用flask shell命令，使用flask shell命令打开的Python Shell自动包含程序上下文，并且已经导入了app实例


上下文（context）可以理解为环境。为了正常运行程序，一些操作相关的状态和数据需要被临时保存下来，这些状态和数据被统称为上下文。在Flask中，上下文有两种，分别为程序上下文和请求上下文

### 项目配置

在Flask中，配置变量就是一些大写形式的Python变量，你也可以称之为配置参数或配置键。使用统一的配置变量可以避免在程序中以硬编码（hard coded）的形式设置程序。

配置变量都通 过Flask对象的app.config属性作为统一的接口来设置和获取，它指向的 Config类实际上是字典的子类，所以你可以像操作其他字典一样操作它

`app.config['ADMIN_NAME'] = 'WangHuaHua'`

配置的名称必须是全大写形式，小写的变量将不会被读取。

使用update（）方法则可以一次加载多个值：

```py
app.config.update(
    TESTING=true,
    SECREATE_KEY="AQ9U3J0IOR2"
)

```

除此之外，你还可以把配置变量存储在单独的Python脚本、JSON 格式的文件或是Python类中，config对象提供了相应的方法来导入配置

### URL与端点

调用url_for（）函数时，第一个参数为端点（endpoint）值。在 Flask中，端点用来标记一个视图函数以及对应的URL规则。端点的默认 值为视图函数的名称

比如，下面的视图函数：
```py
@app.route('/') 
def index():    
    return 'Hello Flask!'
```
这个路由的端点即视图函数的名称index，调用url_for（'index'）即 可获取对应的URL，即“/”。
 
如果URL含有动态部分，那么我们需要在url_for（）函数里传入相应的参数
```py
@app.route('/hello/<name>') 
def greet(name):    
    return 'Hello %s!' % name
```
这时使用url_for（'say_hello'，name='Jack'）得到的URL为“/hello/Jack”。

相对URL只能在程序内部使用。如果你想要生成供外部使用的绝对URL，可以在使用 url_for（）函数时，将_external参数设为True，这会生成完整的URL

### 自定义Flask 命令

通过创建任意一个函数，并为其添加app.cli.command（）装饰器，我们就可以注册一个flask命令。

```py
@app.cli.command():
def hello():
    click.echo('Hello, World')

```

函数的名称即为命令名称，这里注册的命令即hello，你可以使用 flask hello命令来触发函数。作为替代，你也可以在app.cli.command（）装饰器中传入参数来设置命令名称，比如app.cli.command（'say-hello'）会把命令名称设置为say-hello，完整的命令即flask say-hello。

### 静态文件

默认情况下，模板文件存放在项目根目录中的templates文件夹中，静态文件存放在static文件夹下，这两个文件夹需要和包含程序实例的模块处于同一个目录下，对应的项目结构示例如下所示

```
hello/
  template/
  static/
  app.py
```

### Flask与MVC架构

你也许会困惑为什么用来处理请求并生成响应的函数被称为视图函数（viewfunction）”，其实这个命名并不合理。在Flask中，这个命名的约定来自Werkzeug，而Werkzeug中URL匹配的实现主要参考了Routes（一个URL匹配库），再往前追溯，Routes的实现又参考了[Ruby on Rails](http://rubyonrails.org/)。在Rub on Rails中，术语views用来表示MVC（Model-View-Controller，模型-视图-控制器）架构中的View。
MVC架构最初是用来设计桌面程序的，后来也被用于Web程序，应用了这种架构的Web框架有Django、Ruby on Rails等。在MVC架构中，程序被分为三个组件：数据处理（Model）、用户界面（View）、交互逻辑（Controller）。如果套用MVC架构的内容，那么Flask中视图函数的名称其实并不严谨，使用控制器函数（Controller Function）似乎更合适些，虽然它也附带处理用户界面。严格来说，Flask并不是MVC架构的框架，因为它没有内置数据模型支持。
粗略归类，如果想要使用Flask来编写一个MVC架构的程序，那么视图函数可以作为控制器（Controller），视图（View）则是将要学习的使用Jinja2渲染的HTML模板，而模型（Model）可以使用其他库来实现


## Chapter 2 Flask & HTTP

HTTP（Hypertext Transfer Protocol，超文本传输协议）定义了服务器和客户端之间信息交流的格式和传递方式，它是万维网（World Wide Web）中数据交换的基础。


**Flask Web程序工作流程**
当用户访问一个URL，浏览器便生成对应的HTTP请求，经由互联网发送到对应的Web服务器。Web服务器接收请求，通过WSGI将HTTP格式的请求数据转换成我们的Flask程序能够使用的Python数据。在程序中，Flask根据请求的URL执行对应的视图函数，获取返回值生成响应。响应依次经过WSGI转换生成HTTP响应，再经由Web服务器传递，最终被发出请求的客户端接收。浏览器渲染响应中包含的HTML和CSS代码，并执行JavaScript代码，最终把解析后的页面呈现在用户浏览器的窗口中。(这里的服务器指的是处理请求和响应的Web服务器，而不是指物理层面上的服务器主机.)


### HTTP 请求

请求的实质是发送到服务器上的一些数据，这种浏览器与服务器之间交互的数据被称为报文（message），请求时浏览器发送的数据被称为请求报文（request message），而服务器返回的数据被称为响应报文（response message）。

### Request 对象

Flask的Request对象封装了从客户端发来的请求报文，我们能从它获取请求报文中的所有数据。
 

| 属性/方法                                                            | 说明                                                                                                                                                                                      |
| :------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args                                                                 | Werkzeug的ImmutableMultiDict对象,存储解析后的查询字符串,可通过字典方式获取键值.如果想获取未解析的原生查询字符串,可使用query_string                                                        |
| blueprint                                                            | 当前蓝本的名称                                                                                                                                                                            |
| cookies                                                              | 一个包含所有随请求提交的cookies的字典                                                                                                                                                     |
| data                                                                 | 包含字符串形式的请求数据                                                                                                                                                                  |
| endpoint                                                             | 与当前请求相匹配的端点值                                                                                                                                                                  |
| files                                                                | Werkzeug的 MultiDict对象,包含所有上传文件,可以使用字典的形式获取文件.使用的键为文件input标签中的name属性值,对应的值为Werkzeug的FileStorage对象,可以调用save()方法并传入保存路径来保存文件 |
| form                                                                 | Werkzeug的ImmutableMultiDict对象,包含解析后的表单数据.表单字段值通过input标签中的name属性值作为键获取                                                                                     |
| values                                                               | Werkzeug的CombineedMultiDict对象,结合了args和form属性的值                                                                                                                                 |
| get_data(<br>cache=True,<br>as_text=False,<br>parse_from_data=False) | 获取请求中的数据,默认读取为字节字符串(bytestring),将as_text设为True则返回值讲师解码后的unicode字符串                                                                                      |
| get_json(<br>self,force=False,<br>silent=False,<br>cache=True)       | 作为JSON解析并返回数据,如果MIME类型不是JSON,返回None(除非force设为True);解析出错则抛出Werkzeug提供的BadRequest异常,如果silent设为True则返回None;cache设置是否缓存解析后的JSON数据         |
| headers                                                              | 一个Werkzeug的EnvironHeaders对象,包含首部字段,可以以字段的形式操作                                                                                                                        |
| is_json                                                              | 通过MIME类型判断是否为JSON数据,返回布尔值                                                                                                                                                 |
| json                                                                 | 包含解析后的JSON数据,内部调用get_json(),可通过字典的方式获取键值                                                                                                                          |
| method                                                               | 请求的HTTP方法                                                                                                                                                                            |
| referrer                                                             | 请求发起的元URL,即referer                                                                                                                                                                 |
| scheme                                                               | 请求的URL模式(http/https)                                                                                                                                                                 |
| user_agent                                                           | 用户代理                                                                                                                                                                                  |


Werkzeug的MutliDict类是字典的子类，它主要实现了同一个键对应多个值的情况。比如一个文件上传字段可能会接收多个文件。这时就可以通过getlist（）方法来获取文件对象列表。而ImmutableMultiDict类继承了MutliDict类，但其值不可更改

和普通的字典类型不同，当我们从request对象的类型为MutliDict或ImmutableMultiDict的属性（比如files、form、args）中直接使用键作为索引获取数据时（比如request.args['name']），如果没有对应的键，那么会返回HTTP 400错误响应（Bad Request，表示请求无效），而不是抛出KeyError异常。为了避免这个错误，我们应该使用get（）方法获取数据，如果没有对应的值则返回None； get（）方法的第二个参数可以设置默认值，比如 requset.args.get（'name'，'Human'）。

```py

# 示例代码包含安全漏洞，在现实中我们要避免直接将用户传入的数据直接作为响应返回
@app.route('/hello')
def hello():
    name = requets.args.get('name', 'Flask')
    # return '<h1>Hello {}</h1>'.format(name)
    return f'<h1>Hello {name}</h1>'

```

### 在Flask中处理请求

1. 路由匹配

为了便于将请求分发到对应的视图函数，程序实例中存储了一个路由表（app.url_map），其中定义了URL规则和视图函数的映射关系。当请求发来后，Flask会根据请求报文中的URL（path部分）来尝试与这个表中的所有URL规则进行匹配，调用匹配成功的视图函数。如果没有找到匹配的URL规则，说明程序中没有处理这个URL的视图函数，Flask会自动返回404错误响应（Not Found，表示资源未找到）。

。使用flask routes命令可以查看程序中定义的所有路
由，这个列表由app.url_map解析得到：
```
$ flask routes Endpoint  Methods  Rule
 --------  -------  ----------------------
 hello     GET      /hello 
 go_back   GET      /goback/<int:age> 
 hi        GET      /hi 
 ...
  static    GET      /static/<path:filename>
```

2. 设置监听的HTTP方法
在app.route（）装饰器中使用methods参数传入一个包含监听的HTTP方法的可迭代对象。比如，下面的视图函数同时监听GET请求和POST请求：

```py
@app.route('/hello', methods=['GET', 'POST']) 
def hello():
    return '<h1>Hello, Flask!</h1>'
```

3. URL 处理

。URL中的变量部分默认 类型为字符串，但Flask提供了一些转换器可以在URL规则里使用,转换器通过特定的规则指定，即“<转换器：变量名>”。<int：year>把year的值转换为整数，因此我们可以在视图函数中直接对year变量进行数学计算

```py
@app.route('goback/<int:year>')
def go_back(year):
    return '<p>Welcome to %d!</p>'% (2018 - year)
```

**内置的URL变量转换器**
| 转换器 | 说明                         |
| :----- | :--------------------------- |
| string | 不包含斜线的字符串           |
| int    | 整型                         |
| float  | 浮点型                       |
| path   | 包含斜线的字符串             |
| any    | 匹配一系列给定值中的一个元素 |
| uuid   | UUID字符串                   |

4. 请求钩子

有时我们需要对请求进行预处理（preprocessing）和后处理 （postprocessing），这时可以使用Flask提供的一些请求钩子（Hook），它们可以用来注册在请求处理的不同阶段执行的处理函数（或称为回调函数，即Callback）。这些请求钩子使用装饰器实现，通过程序实例app调用，用法很简单：以before_request钩子（请求之前）为例，当你对一个函数附加了app.before_request装饰器后，就会将这个函数注册为before_request处理函数，每次执行请求前都会触发所有before_request处理函数。Flask默认实现的五种请求钩子

| 钩子                  | 说明                                                                                                         |
| :------------------- | :----------------------------------------------------------------------------------------------------------- |
| before_first_request | 注册一个函数,在处理第一个请求前运行                                                                          |
| before_request       | 注册一个函数,在处理每个请求前运行                                                                            |
| after_request        | 注册一个函数,如果没有未处理的异常抛出,会在每个请求结束后运行                                                 |
| teardown_request     | 注册一个函数,即使有未处理的异常抛出,会在每个请求结束后运行,如果发生异常,会传入异常对象作为参数到注册的函数中 |
| after_this_request   | 在视图函数内注册一个函数,会在这个请求结束后运行                                                              |

**请求处理函数调用示意图 page 88**




### 在Flask中生成响应

响应在Flask中使用Response对象表示，响应报文中的大部分内容由服务器处理，大多数情况下，我们只负责返回主体内容。

1. 重定向

对于重定向这一类特殊响应，Flask提供了一些辅助函数。除了像前面那样手动生成302响应，我们可以使用Flask提供的redirect（）函数来生成重定向响应，重定向的目标URL作为第一个参数。

```py
from flask import Flask, redirect

@app.route('/hello')
def hello():
    return redirect('http://www.example.com')
```

使用redirect（）函数时，默认的状态码为302，即临时重定向。如果你想修改状态码，可以在redirect（）函数中作为第二个参数或使用 code关键字传入。

如果要在程序内重定向到其他视图，那么只需在redirect（）函数中使用url_for（）函数生成目标URL即可

```py
from flask import Flask, redirect, url_for
# ... 
@app.route('/hi') 
def hi():    
    # ...    
    return redierct(url_for('hello'))  # 重定向到/hello

@app.route('/hello') 
    def hello(): 
        pass
```

2. 响应格式

不同的响应数据格式需要设置不同的MIME类型，MIME类型在首部的Content-Type字段中定义

MIME类型（又称为media type或content type）是一种用来标识文件类型的机制，它与文件扩展名相对应，可以让客户端区分不同的内容类型，并执行不同的操作。一般的格式为“类型名/子类型名”，其中的子类型名一般为文件扩展名。


### Cookie

HTTP是无状态（stateless）协议。也就是说，在一次请求响应结束后，服务器不会留下任何关于对方状态的信息。但是对于某些Web程序来说，客户端的某些信息又必须被记住，比如用户的登录状态，这样才可以根据用户的状态来返回不同的响应。为了解决这类问题，就有了Cookie技术。Cookie技术通过在请求和响应报文中添加Cookie数据来保存客户端的状态信息。

Cookie指Web服务器为了存储某些数据（比如用户信息）而保存在浏览器上的小型文本数据。浏览器会在一定时间内保存它，并在下一次向同一个服务器发送请求时附带这些数据。Cookie通常被用来进行用户会话管理（比如登录状态），保存用户的个性化信息（比如语言偏好，视频上次播放的位置，网站主题选项等）以及记录和收集用户浏览数据以用来分析用户行为等。

在Flask中，如果想要在响应中添加一个cookie，最方便的方法是使用Response类提供的set_cookie（）方法。要使用这个方法，我们需要先使用make_response（）方法手动生成一个响应对象，传入响应主体作为参数。这个响应对象默认实例化内置的Response类

### Response 类常用属性和方法

|方法/属性|说明|
|:--|:--|
|headers|一个Werkzeug的Headers对象,表示响应首部,可以像字典一样操作|
|status|状态码,文本类型|
|status_code|状态码,整型|
|mimetype|MIME类型|
|set_cookie()|用来设置一个cookie|

Respone类同样拥有和Request 类相同的get_json（）方法、is_json（）方法以及json属性

**set_cookie()方法的参数**

|属性|说明|
|:---|:---|
|key|cookie的键|
|value|cookie的值|
|max_age|cookie被保存的时间数,单位为秒,默认在用户会话结束时过期|
|expires|具体的过期时间,一个datetime对象或一个UNIX时间戳|
|path|限制cookie只在给定的路径可用,默认为整个域名|
|domain|设置cookie可用的域名|
|secure|如果为True,只有通过HTTPS才可以使用|
|httponly|如果为True,禁止客户端JavaScript获取cookie|


set_cookie视图用来设置cookie，它会将URL中的name变量的值设置 到名为name的cookie里

```py
from flask import Flask, make_response 
 
@app.route('/set/<name>') 
def set_cookie(name):    
    response = make_response(redirect(url_for('hello')))    response.set_cookie('name', name)    
    return response
```

在这个make_response（）函数中，我们传入的是使用redirect（）函数生成的重定向响应。set_cookie视图会在生成的响应报文首部中创建一个Set-Cookie字段，即“Set-Cookie：name=Grey；Path=/”。


### session: 安全的Cookie

在编程中，session指用户会话（user session），又称为对话（dialogue），即服务器和客户端/浏览器之间或桌面程序和用户之间建立的交互活动。在Flask中，session对象用来加密Cookie。默认情况下，它会把数据存储在浏览器上一个名为session的cookie里。

1. 设置程序秘钥

程序的密钥可以通过Flask.secret_key属性或配置变量SECRET_KEY
设置，比如：
```py
app.secret_key = 'secret string'

```
更安全的做法是把密钥写进系统环境变量（在命令行中使用export 或set命令），或是保存在.env文件中：
```
SECRET_KEY=secret string
```

然后在程序脚本中使用os模块提供的getenv（）方法获取：
```py
import os 
# ... 
app.secret_key = os.getenv('SECRET_KEY', 'secret string')
```

我们可以在getenv（）方法中添加第二个参数，作为没有获取到对应环境变量时使用的默认值。

2. 模拟用户认证

```py
from flask import redirect, session, url_for

@app.route('/login') def login():    
    session['logged_in'] = True  # 写入session  
    return redirect(url_for('hello'))
```

这个登录视图只是简化的示例，在实际的登录中，我们需要在页面上提供登录表单，供用户填写账户和密码，然后在登录视图里验证账户和密码的有效性。session对象可以像字典一样操作，我们向session中添加一个logged-in cookie，将它的值设为True，表示用户已认证。
当我们使用session对象添加cookie时，数据会使用程序的密钥对其进行签名，加密后的数据存储在一块名为session的cookie里。
你可以在浏览器相应的Content部分看到对应的加密处理后生成的session值。使用session对象存储的Cookie，用户可以看到其加密后的值，但无法修改它。因为session中的内容使用密钥进行签名，一旦数据被修改，签名的值也会变化。这样在读取时，就会验证失败，对应的session值也会随之失效。所以，除非用户知道密钥，否则无法对session cookie的值进行修改。

### Flask 上下文

Flask中有两种上下文，程序上下文（application context）和请求上下文（request context）。

|变量名|类别|说明|
|:---|:---|:---|
|current_app|程序上下文|指向处理请求的当前程序实例|
|g|程序上下文|替代Python的全局变量用法,确保仅在当前请求中可用.用于存储全局数据,每次请求都会重设|
|request|请求上下文|封装客户端发出的请求报文数据|
|session|请求上下文|用于记住请求之间的数据,通过签名的cookie实现|

在下面这些情况下，Flask会自动帮我们激活程序上下文：
 - 当我们使用flask run命令启动程序时。
 - 使用旧的app.run（）方法启动程序时。
 - 执行使用@app.cli.command（）装饰器注册的flask命令时。
 - 使用flask shell命令启动Python Shell时。

当请求进入时，Flask会自动激活请求上下文，这时我们可以使用request和session变量。另外，当请求上下文被激活时，程序上下文也被自动激活。当请求处理完毕后，请求上下文和程序上下文也会自动销毁。也就是说，在请求处理时这两者拥有相同的生命周期。

### 上下文钩子

，Flask也为 上下文提供了一个teardown_appcontext钩子，使用它注册的回调函数会 在程序上下文被销毁时调用，而且通常也会在请求上下文被销毁时调 用。比如，你需要在每个请求处理结束后销毁数据库连接：
```py
@app.teardown_appcontext
def teardown_db(exception):    
    ...    
    db.close()
```

使用app.teardown_appcontext装饰器注册的回调函数需要接收异常对象作为参数，当请求被正常处理时这个参数值将是None，这个函数的返回值将被忽略。


### HTTP 服务器端推送

不论是传统的HTTP请求-响应式的通信模式，还是异步的AJAX式 请求，服务器端始终处于被动的应答状态，只有在客户端发出请求的情
况下，服务器端才会返回响应。这种通信模式被称为客户端拉取 （client pull）。在这种模式下，用户只能通过刷新页面或主动单击加载 按钮来拉取新数据。

然而，在某些场景下，我们需要的通信模式是服务器端的主动推送 （server push）。比如，一个聊天室有很多个用户，当某个用户发送消 息后，服务器接收到这个请求，然后把消息推送给聊天室的所有用户

实现服务器端推送的一系列技术被合称为HTTP Server Push（HTTP 服务器端推送），目前常用的推送技术主要有传统轮询、长轮询、Server-Sent Events

**常用推送技术**

|名称|说明|
|:--|:--|
|传统轮询|在特定的时间间隔内,客户端使用Ajax不断向服务器发送器HTTP请求,然后获取新的数据并更新页面|
|长轮询|和传统轮询类似,但是如果服务器端没有返回数据,那就保持连接一直开启,知道有数据时才返回,取回数据后再次发送另一个请求|
|Server-Sent Events|SSE通过HTML5中的EventSource API实现,SSE会在客户端和服务器端建立一个单项的通道,客户端监听来自服务器端的数据,而服务器端可以在任意时间发送数据,两者建立类似订阅/发布的通信模式|

轮询（polling）这类使用AJAX技术模拟服务器端推送的方法实现起来比较简单，但通常会造成服务器资源上的浪费，增加服务器的负担，而且会让用户的设备耗费更多的电量（频繁地发起异步请求）。SSE效率更高，在浏览器的兼容性方面，除了WindowsIE/Edge，SSE基本上支持所有主流浏览器，但浏览器通常会限制标签页的连接数量。
 
Server-Sent Event的最新标准可以在[WHATWG](https://html.spec.whatwg.org/multipage/server-sentevents.html)查看，浏览器的支持情况可以在[Can I use...](https://caniuse.com/#feat=eventsource)查看

除了这些推送技术，在HTML5的API中还包含了一个WebSocket协议，和HTTP不同，它是一种基于TCP协议的全双工通信协议（fullduplex communication protocol）。和前面介绍的服务器端推送技术相比，WebSocket实时性更强，而且可以实现双向通信（bidirectional communication）。另外，WebSocket的浏览器兼容性要强于SSE

WebSocket协议在[RFC 6455](https://tools.ietf.org/html/rfc6455)中 定义，浏览器的支持情况可以在[Can I use...](https://caniuse.com/#feat=websockets)查看。

如果你想进一步了解这几种推送技术的区别，StackOverflow的这篇答案https://stackoverflow.com/a/12855533/5511849 对这几种推送技术进行了对比，并提供了直观的图示。

### Web 安全方法
对于Web程序的安全问题，一个首要的原则是：永远不要相信你的用户。大部分Web安全问题都是因为没有对用户输入的内容进行“消毒”造成的。

1. 注入攻击
注入攻击包括系统命令（OS Command）注入、SQL（Structured Query Language，结构化查询语言）注入（SQL Injection）、NoSQL注入、ORM（Object Relational Mapper，对象关系映射）注入等。我们这里重点介绍的是SQL注入。
 - 攻击原理
在编写SQL语句时，如果直接将用户传入的数据作为参数使用字符串拼接的方式插入到SQL查询中，那么攻击者可以通过注入其他语句来执行攻击操作，这些攻击操作包括可以通过SQL语句做的任何事：获取 敏感数据、修改数据、删除数据库表……

**主要防范方法**
 - 使用ORM可以一定程度上避免SQL注入问题
 - 验证输入类型。比如某个视图函数接收整型id来查询，那么就在URL规则中限制URL变量为整型。
 - 参数化查询。在构造SQL语句时避免使用拼接字符串或字符串格式化（使用百分号或format（）方法）的方式来构建SQL语句。而要使用各类接口库提供的参数化查询方法
 - 转义特殊字符，比如引号、分号和横线等。使用参数化查询时，各种接口库会为我们做转义工作。
 
2. XSS 攻击

XSS（Cross-Site Scripting，跨站脚本）攻击,Cross-Site Scripting的缩写本应是CSS，但是为了避免和Cascading Style Sheets的缩写产生冲突，所以将Cross（即交叉）使用交叉形状的X表示。

 - 攻击原理
XSS是注入攻击的一种，攻击者通过将代码注入被攻击者的网站中，用户一旦访问网页便会执行被注入的恶意脚本。XSS攻击主要分为反射型XSS攻击（Reflected XSS Attack）和存储型XSS攻击（Stored XSS Attack）两类。


**主要防范措施**
 - HTML转义
防范XSS攻击最主要的方法是对用户输入的内容进行HTML转义，转义后可以确保用户输入的内容在浏览器中作为文本显示，而不是作为代码解析

 - 验证用户输入
XSS攻击可以在任何用户可定制内容的地方进行，例如图片引用、自定义链接。仅仅转义HTML中的特殊字符并不能完全规避XSS攻击，因为在某些HTML属性中，使用普通的字符也可以插入JavaScript代码。除了转义用户输入外，我们还需要对用户的输入数据进行类型验证。在所有接收用户输入的地方做好验证工作。

3. SCRF 攻击

CSRF（Cross Site Request Forgery，跨站请求伪造）

 - 攻击原理
CSRF攻击的大致方式如下：某用户登录了A网站，认证信息保存在cookie中。当用户访问攻击者创建的B网站时，攻击者通过在B网站发送一个伪造的请求提交到A网站服务器上，让A网站服务器误以为请求来自于自己的网站，于是执行相应的操作，该用户的信息便遭到了篡改。总结起来就是，攻击者利用用户在浏览器中保存的认证信息，向对应的站点发送伪造请求。

**主要防范措施**
 - 正确使用HTTP方法
 - CSRF令牌校验
当处理非GET请求时，要想避免CSRF攻击，关键在于判断请求是否来自自己的网站。在前面我们曾经介绍过使用HTTPreferer获取请求来源，理论上说，通过referer可以判断源站点从而避免CSRF攻击，但因为referer很容易被修改和伪造，所以不能作为主要的防御措施。
除了在表单中加入验证码外，一般的做法是通过在客户端页面中加入伪随机数来防御CSRF攻击，这个伪随机数通常被称为CSRF令牌（token）。

除了这几个攻击方式外，我们还有很多安全问题要注意。比如文件上传漏洞、敏感数据存储、用户认证（authentication）与权限管理等。 

你应该列出一个程序安全项目检查清单,可以参考OWASP Top 10 或是CWE(Common Weakness Enumeration，一般弱点列)提供的 [Top 25](https://cwe.mitre.org/top25/)。确保你的程序所有的安全项目检查，也可以使用漏洞检查工具来，比如OWASP提供的[WebScarab](https://github.com/OWASP/OWASP-WebScarab)。

## Chapter 3 Jinja2 模板

完全不想看QAQ, HTML 让前端去写吧!

## Chapter 4 表单验证

## Chapter 5 数据库

数据库一般分为两种，SQL（Structured Query Language，结构化查 询语言）数据库和NoSQL（Not Only SQL，泛指非关系型）数据库

1. SQL
常用的SQL DBMS主要包括SQL Server、Oracle、MySQL、PostgreSQL、SQLite等。关系型数据库使用表来定义数据对象，不同的表之间使用关系连接。

2. NoSQL
NoSQL数据库泛指不使用传统关系型数据库中的表格 形式的数据库。

大型项目通常会同时需要多种数据库，比如使用MySQL作为主数据库存储用户资料和文章，使用Redis（键值对型数据库）缓存数据，使用MongoDB（文档型数据库）存储实时消息。

### ORM

在Web应用里使用原生SQL语句操作数据库主要存在下面两类问题：
 - 手动编写SQL语句比较乏味，而且视图函数中加入太多SQL语句会降低代码的易读性。另外还会容易出现安全问题，比如SQL注入。
 - 常见的开发模式是在开发时使用简单的SQLite，而在部署时切换到MySQL等更健壮的DBMS。但是对于不同的DBMS，我们需要使用不同的Python接口库，这让DBMS的切换变得不太容易。
 

尽管使用ORM可以避免SQL注入问题，但你仍然需要对传入的查询参数进行验证。另外，在执行原生SQL语句时也要注意避免使用字符串拼接或字符串格式化的方式传入参数。
使用ORM可以很大程度上解决这些问题。它会自动帮你处理查询参数的转义，尽可能地避免SQL注入的发生。另外，它为不同的DBMS提供统一的接口，让切换工作变得非常简单。ORM扮演翻译的角色，能够将我们的Python语言转换为DBMS能够读懂的SQL指令，让我们能够使用Python来操控数据库。
 
ORM主要实现了三层映射关系：

 - 表→Python类。
 - 字段（列）→类属性。
 - 记录（行）→类实例。

比如，我们要创建一个contacts表来存储留言，其中包含用户名称和电话号码两个字段。在SQL中，下面的代码用来创建这个表：
```sql
CREATE TABLE contacts(
  name varchar(100) NOT NULL,    
  phone_number varchar(32), 
);
```
如果使用ORM，我们可以使用类似下面的Python类来定义这个表：
```py
from foo_orm import Model, Column, String
class Contact(Model):    
    __tablename__ = 'contacts'    
    name = Column(String(100), nullable=False)    
    phone_number = Column(String(32))
```
要向表中插入一条记录，需要使用下面的SQL语句：
```sql
INSERT INTO contacts(name, phone_number) 
VALUES('Grey Li', '12345678');
```
使用ORM则只需要创建一个Contact类的实例，传入对应的参数表示各个列的数据即可。下面的代码和使用上面的SQL语句效果相同：
```py
contact = Contact(name='Grey Li', phone_number='12345678')
```

ORM还有下面这些优点：
 - 灵活性好。你既能使用高层对象来操作数据库，又支持执行原生 SQL语句。
 - 提升效率。从高层对象转换成原生SQL会牺牲一些性能，但这微 不足道的性能牺牲换取的是巨大的效率提升。
 - 可移植性好。ORM通常支持多种DBMS，包括MySQL、 PostgreSQL、Oracle、SQLite等。你可以随意更换DBMS，只需要稍微 改动少量配置。

使用Python实现的ORM有SQLAlchemy、Peewee、PonyORM等。其 中SQLAlchemy是Python社区使用最广泛的ORM之一

### 使用Flask-SQLAlchemy管理数据库

Flask-SQLAlchemy集成了SQLAlchemy，它简化了连接数据库服务器、管理数据库操作会话等各类工作，让Flask中的数据处理体验变得更加轻松。首先使用Pipenv安装Flask-SQLAlchemy及其依赖

`pipenv install flask-sqlalchemy`

初始化

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# db对象代表我们的数据库，它可以使用Flask-SQLAlchemy提供的所有功能。
db = SQLAlchemy(app)

```

#### 链接数据库

要连接数据库服务器，首先要为我们的程序指定数据库URI（Uniform Resource Identifier，统一资源标识符）。数据库URI是一串包含各种属性的字符串，其中包含了各种用于连接数据库的信息。

**常见的数据库URI格式**

|DBMS|URI|
|:--|:---|
|PostgreSQL|postgresql://username:password@host/databasename|
|MySQL|mysql://username:password@host/databasename|
|Oracle|oracle://username:password@host:port/sidname|
|SQLite(UNIX)|sqlite:////absolute/path/to/foo.db|
|SQLite(Windows)|sqlite:///absolute\\\path\\\to\\\foo.db 或 r'sqlite:///absolute\path\to\foo.db'|
|SQlite(内存型)|sqlite:/// 或 sqlite///:memory|



**配置数据库URI**

```py
import os

app.config['SQLALCHEMY_DATABASE_URI'] = os.getenv('DATABASE_URL', 'sqlite:////' + os.path.join(app.root_path, 'data.db'))
# SQLALCHEMY_TRACK_MODIFICATIONS 决定是否追踪对象的修改
# 关闭警告信息
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

```

在 flask shell 中查看 db 对象

```py
>>> from app import db
>>> db
<SQLAlchemy engine=sqlite:///path/to/your/data.db> 
```

#### 定义数据库模型

用来映射到数据库表的Python类通常被称为数据库模型（model），一个数据库模型类对应数据库中的一个表。定义模型即使用Python类定义表模式，并声明映射关系。所有的模型类都需要继承Flask-SQLAlchemy提供的db.Model基类。

```py
class Note(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    body = db.Column(db.Text)
```
在上面的模型类中，表的字段（列）由db.Column类的实例表示，字段的类型通过Column类构造方法的第一个参数传入。在这个模型中，我们创建了一个类型为db.Integer的id字段和类型为db.Text的body列，分别存储整型和文本.

**常用的字段类型**

|字段|说明|
|:--|:--|
|Integer|整数|
|String|字符串,可选参数length可以用来设置最大长度|
|Text|较长的Unicode文本|
|Date|日期,存储 datetime.date对象|
|Time|时间,,存储 datetime.time对象|
|Datetime|时间和日期,,存储 datetime.datetime对象|
|Interval|时间间隔,,存储 datetime.timedelta对象|
|Float|浮点数|
|Boolean|布尔值|
|PickleType|存储Pickle列化的Python对象|
|LargeBinary|存储任意的二进制数据|

**常用的SQLAlchemy字段参数**

|字段|说明|
|:--|:--|
|primary_key|如果设为True,该字段为主键|
|unique|如果设为True,该字段不允许出现重复值|
|index|如果设为True,为该字段创建索引,以提高查询效率|
|nullable|确定字段值可否为空,值为True或False,默认为True|
|default|为字段设置默认值|

#### 创建数据库和表

创建模型类后，我们需要手动创建数据库和对应的表，也就是我们常说的建库和建表。这通过对我们的db对象调用create_all（）方法实现：
```py shell
# flask shell 
>>> from app import db
>>> db.create_all()
```

通过下面的方式可以查看模型对应的SQL模式（建表语句）
```py
>>> from sqlalchemy.schema import CreateTable 
>>> print(CreateTable(Note.__table__))
CREATE TABLE note (    
  id INTEGER NOT NULL,    
  body TEXT,    
  PRIMARY KEY (id) 
)
```

数据库和表一旦创建后，之后对模型的改动不会自动作用到实际的表中。比如，在模型类中添加或删除字段，修改字段的名称和类型，这时再次调用`create_all（）`也不会更新表结构。如果要使改动生效，最简单的方式是调用`db.drop_all（）`方法删除数据库和表，然后再调用`db.create_all（）`方法创建,我们也可以自己实现一个自定义flask命令完成这个工作
```py
import click

@app.cli.command()
def initdb():
    db.create_all()
    click.echo('Initialized database')

```
在命令行下输入`flask inintdb`即可创建数据库和表：

#### 数据库操作

数据库操作主要是CRUD，即Create（创建）、Read（读取/查询）、Update（更新）和Delete（删除）。

SQLAlchemy使用数据库会话来管理数据库操作，这里的数据库会话也称为事务（transaction）。Flask-SQLAlchemy自动帮我们创建会话，可以通过db.session属性获取。
 

数据库中的会话代表一个临时存储区，你对数据库做出的改动都会存放在这里。你可以调用add（）方法将新创建的对象添加到数据库会话中，或是对会话中的对象进行更新。只有当你对数据库会话对象调用commit（）方法时，改动才被提交到数据库，这确保了数据提交的一致性。另外，数据库会话也支持回滚操作。当你对会话调用rollback（）方法时，添加到会话中且未提交的改动都将被撤销。

默认情况下，Flask-SQLAlchemy（>=2.3.0版本）会自动为模型类生成一个__repr__（）方法。当在PythonShell中调用模型的对象时，__repr__（）方法会返回一条类似“<模型类名主键值>”的字符串，比如<Note 2>。为了便于实际操作测试，示例程序中，所有的模型类都重新定义了__repr__（）方法，返回一些更有用的信息，在实际开发中，这并不是必须的。

```py
class Note(db.Model): 
    ...
    def __repr__(self):
        return '<Note %r>' % self.body
```

1. Create

添加一条新记录到数据库主要分为三步：
1. 创建Python对象（实例化模型类）作为一条记录。
2. 添加新创建的记录到数据库会话。
3. 提交数据库会话。
```py
>>> from app import db, Note 
>>> note1 = Note(body='remember Sammy Jankis') 
>>> note2 = Note(body='SHAVE') 
>>> note3 = Note(body='DON'T BELIEVE HIS LIES, HE IS THE ONE, KILL HIM')
>>> db.session.add(note1) 
>>> db.session.add(note2)
>>> db.session.add(note3) 
>>> db.session.commit()
```

首先从app模块导入db对象和Note类，然后分别创建三个Note实例表示三条记录，使用关键字参数传入字段数据。我们的`Note`类继承自`db.Model`基类，`db.Model`基类会为Note类提供一个构造函数，接收匹配类属性名称的参数值，并赋值给对应的类属性，所以我们不需要自己在Note类中定义构造方法。接着我们调用`add（）`方法把这三个Note对象添加到会话对象db.session中，最后调用`commit（）`方法提交会话。

除了依次调用`add（）`方法添加多个记录，也可以使用`add_all（）`一次添加包含所有记录对象的列表。

在创建模型类实例的时候并没有定义id字段的数据，这是因为主键由SQLAlchemy管理。模型类对象创建后作为临时对象（transient），当你提交数据库会话后，模型类对象才会转换为数据库记录写入数据库中，这时模型类对象会自动获得id值：

Flask-SQLAlchemy提供了一个SQLALCHEMY_COMMIT_ON_TEARDOWN配置变量，将其设为True可以设置自动调用commit（）方法提交数据库会话。因为存在潜在的Bug，目前已不建议使用，而且未来版本中将移除该配置变量。请避免使用该配置变量，可使用手动调用db.session.commit（）方法的方式提交数据库会话。

2. Read

使用模型类提供的query属性附加调用各种过滤方法及查询方法可以从数据库里取回数据

一般来说，一个完整的查询遵循下面的模式：

`<模型类>.query.<过滤方法>.<查询方法>`

**常用的SQLAlchemy查询方法** 

|查询方法|说明|
|:--|:--|
|all()|返回包含所有查询记录的列表|
|first()|返回查询的第一条记录,如果未找到,则返回None|
|one()|返回第一条记录,且仅允许有一条记录,如果记录数量大于1或小于1,则抛出错误|
|get(ident)|传入主键值作为参数,返回指定主键值的记录,如果未找到,则返回None|
|count()|返回查询结果的数量|
|one_or_more()|类似one(),如果结果数量不为1,返回None|
|first_or_404(ident)|返回查询的第一条记录,如果未找到,则返回404错误响应|
|paginate()|返回一个Pagination对象,可以对记录进行分页处理|
|with_parent(instance)|传入模型类实力作为参数,返回和这个实例相关联的对象|

**常用的SQLAlchemy过滤方法**

|查询过滤器方法|说明|
|:--|:--|
|filter()|使用指定的规则过滤记录,返回新产生的查询对象|
|filter_by()|使用指定的规则过滤记录(以关键字表达式的形式),返回新产生的查询对象|
|order_by()|根据指定条件对记录进行排序,返回新产生的查询对象|
|limit(limit)|使用指定的值限定原查询返回的记录数量,返回新产生的查询对象|
|group_by()|根据指定条件对记录进行分组,返回新产生的查询对象|
|offset(offset)|使用指定的值偏移原查询的结果,返回新产生的查询对象|

完整的查询方法和过滤方法列表 在[http://docs.sqlalchemy.org/en/latest/orm/query.html](http://docs.sqlalchemy.org/en/latest/orm/query.html)
完整的可用操作符列表可以访问 [http://docs.sqlalchemy.org/en/latest/core/sqlelement.html#sqlalchemy.sql.operators](http://docs.sqlalchemy.org/en/latest/core/sqlelement.html#sqlalchemy.sql.operators)

3. Update

更新一条记录非常简单，直接赋值给模型类的字段属性就可以改变字段值，然后调用commit（）方法提交会话即可

```py
>>> note = Note.query.get(2)
>>> note.body = 'Lorem ...'
>>> db.session.commit()
```

1. Delete

删除记录和添加记录很相似，不过要把add（）方法换成delete（）方法，最后都需要调用commit（）方法提交修改

```py
>>> note = Note.query.get(2)
>>> db.session.delete(note)
>>> db.session.commit()
```

### 在试图函数里操作数据库

在视图函数里操作数据库的方式和我们在Python Shell中的练习大致相同，只不过需要一些额外的工作。比如把查询结果作为参数传入模板渲染出来，或是获取表单的字段值作为提交到数据库的数据

### 定义关系

在关系型数据库中，我们可以通过关系让不同表之间的字段建立联系。一般来说，定义关系需要两步，分别是创建外键和定义关系属性。在更复杂的多对多关系中，我们还需要定义关联表来管理关系。

#### 配置 flask Python Shell 上下文

在上面的许多操作中，每一次使用flask shell命令启动Python Shell后都要从app模块里导入db对象和相应的模型类。为什么不把它们自动集成到Python Shell上下文里呢？就像Flask内置的app对象一样。这当然可以实现！我们可以使用app.shell_context_processor装饰器注册一个shell上下文处理函数。它和模板上下文处理函数一样，也需要返回包含变量和变量值的字典

```py
@app.shell_context_processor
def make_shell_context():
    return dict(db=db, Note=Note) # 等同于{'db':db,'Note':Note}
```
当你使用flask shell命令启动Python Shell时，所有使用 app.shell_context_processor装饰器注册的shell上下文处理函数都会被自动执行，这会将db和Note对象推送到Python Shell上下文里

#### 一对多
以作者和文章来演示一对多关系：一个作者可以写作多篇文章。

```py
class Author(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(70), unique=True)
  phone = db.Column(db.String(20))
class Article(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  title = db.Column(db.String(70), index=True)
  body = db.Column(db.Text)
```

建立这个一对多关系的目的是在表示作者的Author类中添加一个关系属性articles，作为集合（collection）属性，当我们对特定的Author对象调用articles属性会返回所有相关的Article对象

1. 定义外键
外键是（foreign key）用来在A表存储B表的主键值以便和B表建立联系的关系字段。
因为外键只能存储单一数据（标量），所以外键总是在“多”这一侧定义，多篇文章属于同一个作者，所以我们需要为每篇文章添加外键存储作者的主键值以指向 对应的作者。
在Article模型中，我们定义一个author_id字段作为外键：
```py
class Article(db.Model):
  
  author_id = db.Column(db.Integer, db.ForeignKey('author.id'))
```
使用db.ForeignKey类定义为外键，传入关系另一侧的表名和主键字段名，即author.id。实际的效果是将article表的author_id的值限制为author表的id列的值。它将用来存储author表中记录的主键值

2. 定义关系属性
   
定义关系的第二步是使用关系函数定义关系属性。关系属性在关系的出发侧定义，即一对多关系的“一”这一侧。

```py
class Author(db.Model):
  #...
  articles = db.relationship('Article')
```

关系属性的名称没有限制，你可以自由修改。它相当于一个快捷查询，不会作为字段写入数据库中。
这个属性并没有使用Column类声明为列，而是使用了db.relationship（）关系函数定义为关系属性，因为这个关系属性返回多个记录，我们称之为集合关系属性。relationship（）函数的第一个参数为关系另一侧的模型名称，它会告诉SQLAlchemy将Author类与Article类建立关系。当这个关系属性被调用时，SQLAlchemy会找到关系另一侧（即article表）的外键字段（即author_id），然后反向查询article表中所有author_id值为当前表主键值（即author.id）的记录，返回包含这些记录的列表，也就是返回某个作者对应的多篇文章记录。

```py
>>> foo = Author(name='Foo') 
>>> spam = Article(title='Spam') 
>>> ham = Article(title='Ham')
>>> db.session.add(foo) 
>>> db.session.add(spam) 
>>> db.session.add(ham)
```

3. 建立关系
   
建立关系有两种方式，第一种方式是为外键字段赋值，比如：
```py
>>> spam.author_id = 1 
>>> db.session.commit()
```

将spam对象的author_id字段的值设为1，这会和id值为1的Author对象建立关系。提交数据库改动后，如果我们对id为1的foo对象调用articles关系属性，会看到spam对象包括在返回的Article对象列表中

另一种方式是通过操作关系属性，将关系属性赋给实际的对象即可建立关系。集合关系属性可以像列表一样操作，调用append（）方法来与一个Article对象建立关系：
```py
>>> foo.articles.append(spam) 
>>> foo.articles.append(ham) 
>>> db.session.commit()
```

外键字段由SQLAlchemy管理，我们不需要手动设置。当通过关系属性建立关系后，外键字段会自动获得正确的值。

在关系函数中，有很多参数可以用来设置调用关系属性进行查询时的具体行为。
**常用的SQLAlchemy关系函数参数**

|参数名|说明|
|:---|:---|
|back_populates|定义反向引用,用于建立双向关系,在关系的另一侧也必须显示定义关系属性|
|backref|添加反向引用,自动在另一侧建立关系属性,是back_populates的简化版|
|lazy|指定如何加载相关记录|
|uselist|指定是否使用列表的形式加载记录|
|cascade|设置级联操作|
|order_by|指定加载相关记录时的排序方式|
|secondary|在多对多关系中指定关联表|
|primaryjoin|指定多对多关系中的一级联结条件|
|secondaryjoin|指定多对多关系中的一级联结条件|

**常用的SQLAlchemy关系记录加载方式（lazy参数可选值）**
|参数名|说明|
|:---|:---|
|select|在必要时一次性加载记录,返回包含记录的列表,等同于lazy=True|
|joined|和父查询一样加载记录,但是用联结,等同于lazy=False|
|immediate|一旦父查询加载就加载|
|subquery|类似于joined,不过将使用子查询|
|dynamic|不直接加载记录,而是返回一个包含相关记录的query对象,以便在继续附加查询函数对结果进行过滤|

大多数情况下我们只需要使用默认值（select），只有在调用关系属性会返回大量记录，并且总是需要对关系属性返回的结果附加额外的查询时才需要使用动态加载（lazy='dynamic'）。

4. 建立双向关系

在某些情况下，你也许希望能在Article类中定义一个类似的author关系属性，当被调用时返回对应的作者记录，这类返回单个值的关系属性被称为标量关系属性。而这种两侧都添加关系属性获取对方记录的关系我们称之为双向关系（bidirectional relationship）。

双向关系的建立很简单，通过在关系的另一侧也创建一个relationship（）函数，我们就可以在两个表之间建立双向关系。我们使用作家（Writer）和书（Book）的一对多关系来进行演示，建立双向关系后的Writer和Book类
```py
class Writer(db.Model):    
  id = db.Column(db.Integer, primary_key=True)    
  name = db.Column(db.String(70), unique=True)    
  books = db.relationship('Book', back_populates='writer')
class Book(db.Model):    
  id = db.Column(db.Integer, primary_key=True)    
  title = db.Column(db.String(50), index=True)    
  writer_id = db.Column(db.Integer, db.ForeignKey('writer.id'))    
  writer = db.relationship('Writer', back_populates='books')
```

在“多”这一侧的Book（书）类中，我们新创建了一个writer关系属性，这是一个标量关系属性，调用它会获取对应的Writer（作者）记录；而在Writer（作者）类中的books属性则用来获取对应的多个Book（书）记录。在关系函数中，我们使用back_populates参数来连接对方，back_populates参数的值需要设为关系另一侧的关系属性名。

5. 使用backref简化关系定义
  
以一对多关系为例，backref参数用来自动为关系另一侧添加关系属性，作为反向引用（back reference），赋予的值会作为关系另一侧的关系属性名称

使用歌手（Singer）和歌曲（Song）的一对多关系作为演示，分别创建Singer和Song类
```py
class Singer(db.Model):    
    id = db.Column(db.Integer, primary_key=True)    
    name = db.Column(db.String(70), unique=True)    
    songs = db.relationship('Song', backref='singer')
class Song(db.Model):    
    id = db.Column(db.Integer, primary_key=True)    
    name = db.Column(db.String(50), index=True)    
    singer_id = db.Column(db.Integer, db.ForeignKey('singer.id'))
```

在定义集合属性songs的关系函数中，我们将backref参数设为 singer，这会同时在Song类中添加了一个singer标量属性。这时我们仅需 要定义一个关系函数，虽然singer是一个“看不见的关系属性”，但在使 用上和定义两个关系函数并使用back_populates参数的效果完全相同。

需要注意的是，使用backref允许我们仅在关系一侧定义另一侧的关 系属性，但是在某些情况下，我们希望可以对在关系另一侧的关系属性 进行设置，这时就需要使用backref（）函数。backref（）函数接收第一 个参数作为在关系另一侧添加的关系属性名，其他关键字参数会作为关 系另一侧关系函数的参数传入。比如，我们要在关系另一侧“看不见的 relationship（）函数”中将uselist参数设为False，可以这样实现：
```py
class Singer(db.Model):    
  ...    
  songs = relationship('Song', backref=backref('singer', uselist=False))
  ```
  
  尽管使用backref非常方便，但通常来说“显式好过隐式”，所以我们 应该尽量使用back_populates定义双向关系

### 多对一

关系属性在关系模式的出发侧定义。当出发点在“多”这一侧时，我们希望在Citizen类中添加一个关系属性city来获取对应的城市对象，因为这个关系属性返回单个值，我们称之为标量关系属性。在定义关系时，外键总是在“多”这一侧定义，所以在多对一关系中外键和关系属性都定义在“多”这一侧

```py
class Citizen(db.Model):    
    id = db.Column(db.Integer, primary_key=True)    
    name = db.Column(db.String(70), unique=True)    
    city_id = db.Column(db.Integer, db.ForeignKey('city.id')) city = db.relationship('City')
class City(db.Model):    
    id = db.Column(db.Integer, primary_key=True)    
    name = db.Column(db.String(30), unique=True)

```

这时定义的city关系属性是一个标量属性（返回单一数据）。当Citizen.city被调用时，SQLAlchemy会根据外键字段city_id存储的值查找对应的City对象并返回，即居民记录对应的城市记录。
当建立双向关系时，如果不使用backref，那么一对多和多对一关系模式在定义上完全相同，这时可以将一对多和多对一视为同一种关系模式。

### 一对一

一对一关系实际上是通过建立双向关系的一对多关系的基础上转化而来。我们要确保关系两侧的关系属性都是标量属性，都只返回单个值，所以要在定义集合属性的关系函数中将uselist参数设为False，这时一对多关系将被转换为一对一关系

```py
class Country(db.Model):    
    id = db.Column(db.Integer, primary_key=True)    
    name = db.Column(db.String(30), unique=True)    
    capital = db.relationship('Capital', uselist=False)
class Capital(db.Model):    
    id = db.Column(db.Integer, primary_key=True)    
    name = db.Column(db.String(30), unique=True)    
    country_id = db.Column(db.Integer, db.ForeignKey('country.id'))    
    country = db.relationship('Country')

```

### 多对多

使用学生和老师来演示多对多关系：每个学生有多个老师，而每个老师有多个学生

在多对多关系中，每一个记录都可以与关系另一侧的多个记录建立关系，关系两侧的模型都需要存储一组外键。在SQLAlchemy中，要想表示多对多关系，除了关系两侧的模型外，我们还需要创建一个关联表（association table）。关联表不存储数据，只用来存储关系两侧模型的外键对应关系

```py
association_table = db.Table(
  'association',
  db.Column(
    'student_id', 
    db.Integer, 
    db.ForeignKey('student')
    )
  )

class Student(db.Model):    
  id = db.Column(db.Integer, primary_key=True)    
  name = db.Column(db.String(70), unique=True)    
  grade = db.Column(db.String(20))    
  teachers = db.relationship(
              'Teacher',                       secondary=association_table,        back_populates='students')
class Teacher(db.Model):    
  id = db.Column(db.Integer, primary_key=True)    
  name = db.Column(db.String(70), unique=True)    
  office = db.Column(db.String(20))

```

关联表使用db.Table类定义，传入的第一个参数是关联表的名称。我们在关联表中定义了两个外键字段：teacher_id字段存储Teacher类的主键，student_id存储Student类的主键。借助关联表这个中间人存储的外键对，我们可以把多对多关系分化成两个一对多关系

### 更新数据库

1. 重新生成表
如果你并不在意表中的数据，最简单的方法是使用drop_all（）方法删除表以及其中的数据，然后再使用create_all（）方法重新创建

```py
>>> db.drop_all()
>>> db.create_all()
```
```py
@app.cli.command() 
@click.option('--drop', is_flag=True, help='Create after drop.') 
def initdb(drop):    
  """Initialize the database."""    
  if drop:         
    db.drop_all()        
  db.create_all()    
  click.echo('Initialized database.')
```

使用click提供的option装饰器为命令添加了一个--drop选项，将is_flag参数设为True可以将这个选项声明为布尔值标志（boolean flag）。--drop选项的值作为drop参数传入命令函数，如果提供了这个选项，那么drop的值将是True，否则为False

### 使用Flask-Migrate迁移数据库

SQLAlchemy的开发者Michael Bayer写了一个数据库迁移工具 ——Alembic来帮助我们实现数据库的迁移，数据库迁移工具可以在不 破坏数据的情况下更新数据库表的结构。

扩展Flask-Migrate集成了Alembic，提供了一些flask命令来简化迁移工作，我们将使用它来迁移数据库

`pipenv install flask-migrate`

1. 初始化Migrate类

```py
# app.py
from flask_migrate import Migrate
# ...
migrate = Migrate(app,db)

```

2. 创建迁移环境

`flask db init`

Flask-Migrate提供了一个命令集，使用db作为命名集名称，它提供的命令都以flask db开头。你可以在命令行中输入flask --help查看所有可用的命令和说明。
3. 生成迁移脚本

`flask db migrate -m "add note timestamp"`

这条命令可以简单理解为在flask里对数据库（db）进行迁移（migrate）。-m选项用来添加迁移备注信息

迁移命令是由Alembic自动生成的，其中可能包含错误，所以有必要在生成后检查一下。
因为每一次迁移都会生成新的迁移脚本，而且Alembic为每一次迁移都生成了修订版本（revision）ID，所以数据库可以恢复到修改历史中的任一点。正因为如此，迁移环境中的文件也要纳入版本控制。
有些复杂的操作无法实现自动迁移，这时可以使用revision命令手动创建迁移脚本。这同样会生成一个迁移脚本，不过脚本中的upgrade（）和downgrade（）函数都是空的。你需要使用Alembic提供的Operations对象指令在这两个函数中实现具体操作，具体可以访问Alembic官方文档查看。


4. 更新数据库

使用upgrade子命令即可更新数据库,如果还没有创建数据库和表，这个命令会自动创建；如果已经创建，则会在不损坏数据的前提下执行更新。
 

`flask db upgrade`

如果你想回滚迁移，那么可以使用downgrade命令（降级），它会撤销最后一次迁移在数据库中的改动，这在开发时非常有用。比如，当你执行upgrade命令后发现某些地方出错了，这时就可以执行
`flask db downgrade` 
命令进行回滚，删除对应的迁移脚本，重新生成迁移脚本后再进行更新（upgrade）
 
### 数据库进阶操作(未整理)

### Flask-Mail 发送电子邮件

扩展Flask-Mail包装了Python标准库中的smtplib包，简化了在Flask程序中发送电子邮件的过程

`pipenv install flask-mail`

1. 初始化

```py
from flask_mail import Mail

mail = Mail(app)
```

2. 配置 Flask-Mail

Flask-Mail通过连接SMTP（Simple Mail Transfer Protocol，简单邮件传输协议）服务器来发送邮件。因此，在开始发送电子邮件前，我们需要配置SMTP服务器。
默认的邮件服务器配置即为localhost，端口为25。在开发和测试阶段，我们可以使用邮件服务提供商的SMTP服务器（比如Gmail），这时我们需要对Flask-Mail进行配置。

**Flask-Mail的常用配置**
|配置键|说明|默认值|
|MAIL_SERVER|用于发送邮件的SMTP服务器|localhost|
|MAIL_PORT|发信端口|25|
|MAIL_USE_TLS|是否使用STARTTLS|False|
|MAIL_USE_SSL|是否使用SSL/TLS|False|
|MAIL_USERNAME|发信服务器的用户名|None|
|MAIL_PASSWORD|发信服务器的密码|None|
|MAIL_DEFAULT_SENDER|默认的发信人|None|

对发送的邮件进行加密可以避免邮件在发送过程中被第三方截获和篡改。SSL（Security Socket Layer，安全套接字层）和TLS（Transport Layer Security，传输层安全）是两种常用的电子邮件安全协议。TLS继承了SSL，并在SSL的基础上做了一些改进（换句话说，TLS是后期版 本的SSL）。所以，在大多数情况下，名词SSL和TLS可以互换使用。 它们通过将MAIL_USE_SSL设置为True开启。STARTTLS是另一种加密方式，它会对不安全的连接进行升级（使用SSL或TLS）。尽管它的名字中包含TLS，但也可能会使用SSL加密。根据加密的方式不同，端口也要相应改变

```py
# SSL/TLS加密：
MAIL_USE_SSL = True 
MAIL_PORT = 465
# STARTTLS加密
MAIL_USE_TLS = True 
MAIL_PORT = 587
```

**常用SMTP服务提供商配置**

|服务商|MAIL_SERVER|MAIL-USERNAME|MAIL-PASSWORD|额外步骤|
|:---|:---|:---|:---|:---|
|Gmail|smtp.gmail.com|邮箱地址|邮箱密码|开启"Allow less seccure apps", 在本地设置VPN代理|
|qq邮箱|smtp.qq.com|邮箱地址|授权码|开启SMTP服务并获取授权码|
|sina邮箱|smtp.sina.com|邮箱地址|邮箱密码|开启SMTP服务|
|163邮箱|smtp.163.com|邮箱地址|授权码|开启SMTP服务并获取授权码|

163邮箱的SMTP服务器不支持STARTTLS，你需要使用SSL/TLS加密。具体来说，需要将MAIL_USE_SSL设为True，MAIL_PORT设为 465。

Gmail、Outlook、QQ邮箱等这类服务被称为EPA（Email Service Provider），只适用于个人业务使用，不适合用来发送事务邮件 （Transactional Email）。对于需要发送大量邮件的事务性邮件任务，更好的选择则是使用自己配置的SMTP服务器或是使用类似SendGrid、 Mailgun的事务邮件服务提供商（Transactional Email Service）

邮件服务器配置

```py
#app.py
app.config.update(
 MAIL_SERVER = os.getenv('MAIL_SERVER')    
 MAIL_PORT = 587    
 MAIL_USE_TLS = True    
 MAIL_USERNAME = os.getenv('MAIL_USERNAME')    
 MAIL_PASSWORD = os.getenv('MAIL_PASSWORD')    
 #  默认发信人由一个两元素元组组成，即(姓名，邮箱地址)
 MAIL_DEFAULT_SENDER = ('USERNAME', os.getenv('MAIL_USERNAME')) 
)

mail = Mail(app)
```

```py
# .env
MAIL_SERVER=smtp.example.com MAIL_USERNAME=yourusername@example.com MAIL_PASSWORD=your_password
```
在实例化Mail类时，Flask-Mail会获取配置以创建一个用于发信的对象，所以确保在实例化Mail类之前加载配置。

需要注意，使用邮件服务提供商提供的SMTP服务器发信时，发信人字符串中的邮件地址必须和邮箱地址相同。你可以直接使用MAIL_USERNAME的值构建发信人地址：

设置默认发信人后，在发信时就可以不用再指定发信人。

### 构建邮件数据

一封邮件至少要包含主题、收件人、正文、发信人这几个元素。发信人（sender）在前面我们已经使用MAIL_DEFAULT_SENDER配置变量指定过了，剩下的分别通过Message类的构造方法中的subject、 recipients、body关键字传入参数，其中recipients为一个包含电子邮件地址的列表。

```py
from flask_mail import Message

message = Message(
  subject="",
    # 收信人字符串可以为两种形式：
    # 'Zorn<zorn@example.com>'或'zorn@example.com'。
  recipients=[
    'Zorn<zorn@example.com>'
  ],
  body=""
)

```

### 发送邮件
通过对mail对象调用send（）方法，传入我们在上面构建的邮件对象即可发送邮件：

```py
>>> mail.send(message)
```
为了方便重用，我们把这些代码包装成一个通用的发信函数send_mail（）

```py

def send_mail(subject, to, body):    
  message = Message(subject, recipients=[to], body=body)    mail.send(message)
```

### 使用事务邮件服务SendGird

### 异步发送邮件
当使用SMTP的方式发送电子邮件时，如果你手动使用浏览器测试程序的注册功能，你可能会注意到，在提交注册表单后，浏览器会有几秒钟的不响应。因为这时候程序正在发送电子邮件，发信的操作阻断了请求——响应循环，直到发信的send_mail（）函数调用结束后，视图函数才会返回响应。这几秒的延迟带来了不好的用户体验，为了避免这个延迟，我们可以将发信函数放入后台线程异步执行

```py
from threading import Thread 
... 
def _send_async_mail(app, message):    
    with app.app_context():        
        ail.send(message)
def send_mail(subject, to, body):    
    message = Message(subject, recipients=[to], body=body)    thr = Thread(target=_send_async_mail, args=[app, message])thr.start()    
    return thr


```
因为Flask-Mail的send（）方法内部的调用逻辑中使用了current_app变量，而这个变量只在激活的程序上下文中才存在，这里在后台线程调用发信函数，但是后台线程并没有程序上下文存在。为了正常实现发信功能，我们传入程序实例app作为参数，并调用app.app_context（）手动激活程序上下文。

在生产环境下，我们应该使用异步任务队列处理工具来处理这类任务，比如[Celery](http://www.celeryproject.com) 。



## 蓝图

1. 创建蓝图

```py
# blog.py
from flask import Blueprint

blog_bp = Blueprint('blog', __name__)
# Blueprint(蓝图的名称, 包/模块的名称)
```
**错误处理函数**
使用蓝图实例的errorhandler（）装饰器可以把错误处理器注册到蓝图上，这些错误处理器只会捕捉访问该蓝图中的路由发生的错误；使用蓝图实例的app_errorhandler（）装饰器则可以注册一个全局的错误处理器。
**请求处理函数**
在蓝图中，使用before_request、after_request、teardown_request等装饰器注册的请求处理函数是蓝图独有的，也就是说，只有该蓝图中的视图函数对应的请求才会触发相应的请求处理函数。另外，在蓝图中也可以使用before_app_request、after_app_request、teardown_app_request、before_app_first_request方法，这些方法注册的请求处理函数是全局的

**模板上下文处理函数**
和请求钩子类似，蓝本实例可以使用context_processor装饰器注册蓝本特有的模板上下文处理器；使用app_context_processor装饰器则会注册程序全局的模板上下文处理器。
另外，蓝本对象也可以使用app_template_global（）、app_template_filter（）和app_template_test（）装饰器，分别用来注册全局的模板全局函数、模板过滤器和模板测试器。
 
1. 注册蓝图
```py
# __init__.py
from myblog.views.blog import blog_bp

app.register_blueprint(blog_bp)
```
Flask.register_blueprint（）方法注册，必须传入的参数是我们在上面创建的蓝本对象。


