## Django教程

https://www.liujiangblog.com/course/django/84

https://code.ziqiangxuetang.com/django/django-tutorial.html

### 1. 简介

Django是一个由Python编写的开源Web框架。

Django基于`MVC`架构，即Model（模型）+View（视图）+ Controller（控制器）设计模式，因此天然具有`MVC`的出色基因：开发快捷、部署方便、可重用性高、维护成本低等优点。

#### 1.1 MVC及MTV设计模式

- `MVC`设计模式

  `MVC`把Web框架分为三个基础部分：

  - 模型（Model）：用于封装与应用程序的业务逻辑相关的数据及对数据的处理方法，是Web应用程序中用于处理应用程序的数据逻辑的部分，Model只提供功能性的接口，通过这些接口可以获取Model的所有功能。白话说，这个模块就是业务逻辑和数据库的交互层，定义了数据表。
  - 视图（View）：负责数据的显示和呈现，是对用户的直接输出。
  - 控制器（Controller）：负责从用户端收集用户的输入，可以看成提供View的反向功能。

  这三个部分互相独立，但又相互联系，使得改进和升级界面及用户交互流程，在Web开发过程任务分配时，不需要重写业务逻辑及数据访问代码。

- `MTV`设计模式

  Django对传统的MVC设计模式进行了修改，将视图分成View模块和Template模块两部分，将动态的逻辑处理与静态的页面展示分离开。而Model采用了ORM技术，将关系型数据库表抽象成面向对象的Python类，将数据库的表操作转换成Python的类操作，避免了编写复杂的SQL语句。

  - 模型（Model）：和MVC中的定义一样。
  - 模板（Template）：将模型数据与HTML页面结合起来的引擎。
  - 试图（View）：负责实际的业务逻辑实现。

  Django的MTV模型组织可参考下图所示：

  <img src="D:\file\Django\MTV.png" alt="MTV" style="zoom: 80%;" />

### 2. 安装

Django是基于Python的Web框架，依赖Python环境，所以需要提前安装好Python解释器。

Django各版本对Python版本的依赖关系如下表所示：

| Django版本 | Python版本                         |
| ---------- | ---------------------------------- |
| 1.11       | 2.7，3.4，3.5，3.6，3.7（1.11.17） |
| 2.0        | 3.4，3.5，3.6，3.7                 |
| 2.1        | 3.5，3.6，3.7                      |
| 2.2        | 3.5，3.6，3.7，3.8（2.2.8）        |
| 3.0        | 3.6，3.7，3.8                      |
| 3.1        | 3.6，3.7，3.8                      |

#### 2.1 通过pip安装Django

```shell
pip install django==2.2
```

#### 2.2 验证安装

```shell
python -c "import django"
```

### 3. 创建项目

```shell
$ django-admin startproject law_search_webstation

law_search_webstation
├── db.sqlite3    # 数据库，首次运行自动生成
├── law_search_webstation    # 项目文件目录，它的名字是你引用内部文件的Python包名，例如：law_search_webstation.urls
│   ├── __init__.py    # 一个定义包的空文件
│   ├── __pycache__
│   ├── settings.py    # 项目配置文件
│   ├── urls.py        # 路由文件，所有的任务都是从这里开始分配，相当于Django驱动站点的目录
│   └── wsgi.py        # 一个基于WSGI的web服务器进入点，提供底层的网络通信功能，通常不用关心
└── manage.py    # 一个命令行工具，管理Django的交互脚本
```

回到根目录下，运行`python manage.py runserver`，Django会以`127.0.0.1:8000`这个默认配置启动开发服务器。

```shell
$ python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

June 13, 2021 - 09:14:56
Django version 2.2, using settings 'law_search_webstation.settings'
Starting development server at http://127.0.0.1:8080/
Quit the server with CONTROL-C.
```

打开你的浏览器，在地址栏输入`127.0.0.1:8000`，如果看到如下的界面，说明Django一切正常，你可以开始Django之旅了！

<img src="D:\file\Django\django_startup.png" alt="django_startup" style="zoom:80%;" />

**Django提供了一个用于开发的web服务器，使你无需配置一个类似Ngnix的生产服务器，就能让站点运行起来。**这是一个由Python编写的轻量级服务器，简易并且不安全，因此不要将它用于生产环境。

**注意，第三方访问还需要修改`law_search_webstation/law_search_webstation/setting.py`文件：**

```python
ALLOWED_HOSTS = ['*']
```

修改服务器的ip地址：

```shell
$ python manage.py runserver 0:8080
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.


June 13, 2021 - 09:14:56
Django version 2.2, using settings 'law_search_webstation.settings'

Starting development server at http://0:8080/
Quit the server with CONTROL-C.
```

0 是 0.0.0.0 的简写，Django将运行在0.0.0.0:8080上，整个局域网内都将可以访问站点，而不只是是本机

在浏览器输入`\<IP_ADDRESS\>:8080`回车，便可访问该站点。

**注意，Django的开发服务器具有自动重载功能，当你的Python代码有修改，服务器会在一个周期后自动更新。**但是，有一些动作，比如增加文件，不会触发服务器重载，这时就需要你自己手动重启。所以建议，在任何修改代码的操作后，手动重启开发服务器，确保修改被应用。

### 4. 创建应用

在 Django 中，每一个应用（app）都是一个 Python 包，并且遵循着相同的约定。Django 自带一个工具，可以帮你生成应用的基础目录结构。

app应用与project项目的区别：

- 一个app实现某个具体功能，比如博客、公共档案数据库或者简单的投票系统；
- 一个project是配置文件和多个app的集合，这些app组合成整个站点；
- 一个project可以包含多个app；
- 一个app可以属于多个project！

app的存放位置可以是任何地点，但是通常都将它们放在与`manage.py`脚本同级的目录下，这样方便导入文件。

#### 4.1 顶层设计

该应用包括以下几个部分：

- 一个可以让公众用户进行检索（律所 + 律师）的站点

#### 4.2 创建app

```shell
$ python manage.py startapp app

# 系统会自动生成app应用的目录，其结构如下：
law_search_webstation
├── app
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── db.sqlite3
├── law_search_webstation
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── settings.cpython-36.pyc
│   │   ├── urls.cpython-36.pyc
│   │   └── wsgi.cpython-36.pyc
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

### 5. 请求与相应

#### 5.1 编写视图

下面，我们编写一个视图，也就是具体的业务代码。

在`app/views.py`文件中，编写代码：

```python
from django.shortcuts import render
from django.shortcuts import HttpResponse

# Create your views here.
def index(request):
```

为了调用该视图，我们还需要编写urlconf，也就是路由配置。在app目录中新建一个文件，名字为`urls.py`（不要换成别的名字），在其中输入代码如下：

```python
from django.urls import path

from . import views

app_name = 'app'

urlpatterns = [
    path('', views.index, name='index'),
]
```

接下来，在项目的**主urls.py文件**中添加`urlpattern`条目，指向我们刚才建立的app独有的urls.py文件，这里需要导入include模块。打开`mysite/urls.py`文件，代码如下：

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', include('app.urls'))
]
```

include语法相当于多级路由，它把接收到的url地址去除与此项匹配的部分，将剩下的字符串传递给下一级路由urlconf进行判断。

include的背后是一种即插即用的思想。项目的根路由不关心具体app的路由策略，只管往指定的二级路由转发，实现了应用解耦。app所属的二级路由可以根据自己的需要随意编写，不会和其它的app路由发生冲突。app目录可以放置在任何位置，而不用修改路由。这是软件设计里很常见的一种模式。

**建议**：除了admin路由外，尽量给每个app设计自己独立的二级路由。

#### 5.2 path()方法

一个路由配置模块就是一个urlpatterns列表，列表的每个元素都是一项path，每一项path都是以path()的形式存在。

path()方法可以接收4个参数，其中前2个是必须的：`route`和`view`，以及2个可选的参数：`kwargs`和`name`。

- `route`

  route 是一个匹配 URL 的准则（类似正则表达式）。当 Django 响应一个请求时，它会从 urlpatterns 的第一项path开始，按顺序依次匹配列表中的项，直到找到匹配的项，然后执行该条目映射的视图函数或下级路由，其后的条目将不再继续匹配。因此，url路由执行的是短路机制，path的编写顺序非常重要！

- `view`

  view指的是处理当前url请求的视图函数。当Django匹配到某个路由条目时，自动将封装的`HttpRequest`对象作为第一个参数，被“捕获”的参数以关键字参数的形式，传递给该条目指定的视图view。

- `kwargs`

  任意数量的关键字参数可以作为一个字典传递给目标视图。

- `name`

  对你的URL进行命名，让你能够在Django的任意处，尤其是模板内显式地引用它。这是一个非常强大的功能，相当于给URL取了个全局变量名，不会将url匹配地址写死。

**===== modify settings.py =====**

(1) 将`TIME_ZONE`设置为国内所在的时区`Asia/Shanghai`，这样显示的就是我们北京时间

(2) settings文件中顶部的`INSTALLED_APPS`设置项。它列出了所有的项目中被激活的Django应用（app）。你必须将你自己创建的app注册在这里。每个应用可以被多个项目使用，并且可以打包和分发给其他人在他们的项目中使用。

默认情况，`INSTALLED_APPS`中会自动包含下列条目，它们都是Django自动生成的：

- django.contrib.admin：admin管理后台站点
- django.contrib.auth：身份认证系统
- django.contrib.contenttypes：内容类型框架
- django.contrib.sessions：会话框架
- django.contrib.messages：消息框架
- django.contrib.staticfiles：静态文件管理框架

上面的那些应用会默认被启动，并且也需要建立一些数据库表，所以在使用它们之前我们要在数据库中创建这些表。

```shell
$ python manage.py migrate
```

migrate命令将遍历`INSTALLED_APPS`设置中的所有项目，在数据库中创建对应的表，并打印出每一条动作信息。

**============ end =============**

### 6. 模型与后台

#### 6.1 数据库配置

Django默认使用SQLite3数据库，因为Python原生支持SQLite3数据库，所以你无须安装任何程序，就可以直接使用它。

#### 6.2 admin后台管理站点

Django最大的优点之一，就是体贴的为你提供了一个基于项目model创建的一个后台管理站点admin。这个界面只给站点管理员使用，并不对大众开放。虽然admin的界面可能不是那么美观，功能不是那么强大，内容不一定符合你的要求，但是它是免费的、现成的，并且还是可定制的，有完善的帮助文档，那么，你还要什么自行车？

##### 6.2.1 创建管理员用户

```shell
$ python manage.py createsuperuser

Username (leave blank to use 'ubuntu'): dxliu
Email address: 952010616@qq.com
Password:
Password (again):
This password is too short. It must contain at least 8 characters.
This password is entirely numeric.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
```

##### 6.2.2 启动开发服务器

执行runserver命令启动服务器后，在浏览器访问`http://127.0.0.1:8000/admin/`。你就能看到admin的登陆界面了：

<img src="D:\file\Django\admin.png" alt="admin" style="zoom:80%;" />

### 7. 视图和模板

Django中视图的概念是一类具有相同功能和模板的网页的集合。一个视图通常对应一个页面，提供特定的功能，使用特定的模板。

在Django中，网页和其它的一些内容都是通过视图来处理的。视图其实就是一个简单的Python函数（在基于类的视图中称为方法）。Django通过对比请求的URL地址来选择对应的视图，也就是路由。

为了将 URL 和视图关联起来，Django 使用 `URLconfs`来配置路由。

#### 7.1 编写视图

每个视图至少做两件事之一：返回一个包含请求页面的HttpResponse对象或者弹出一个类似Http404的异常。

**这里有个非常重要的问题：目前视图中的HTML页面是硬编码的。**如果你想改变页面的显示内容，就必须修改这里的Python代码。为了解决这个问题，需要使用Django提供的模板系统，解耦视图和模板之间的硬连接。

~~首先，在`app`目录下创建一个新的`templates`目录，Django会在它里面查找模板文件。~~

~~项目`settings.py`文件中的 `TEMPLATES`配置项描述了 Django 如何载入和渲染模板。默认的设置文件设置了 `DjangoTemplates` 后端作为模板引擎，并将 `APP_DIRS`设置成了 True。这一选项将会让 `DjangoTemplates` 在每个 `INSTALLED_APPS` 文件夹中寻找 "`templates`" 子目录。~~

~~在刚才创建的`templates`目录中，再创建一个新的子目录名叫`app`，进入该子目录，创建一个新的HTML文件`index.html`。换句话说，你的模板文件应该是`app/templates/app/index.html`。因为 Django 会寻找到对应的`app_directories` ，所以你只需要使用`app/index.html`就可以引用到这一模板了。~~

首先，在根目录下创建一个新的`templates`目录，Django会在它里面查找模板文件。

项目`settings.py`文件中的 `TEMPLATES`配置项描述了 Django 如何载入和渲染模板。默认的设置文件设置了 `DjangoTemplates` 后端作为模板引擎，并将 `APP_DIRS`设置成了 True。这一选项将会让 `DjangoTemplates` 在每个 `INSTALLED_APPS` 文件夹中寻找 "`templates`" 子目录，这里修改为：

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],    # 设置为根目录下templates
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

在刚才创建的`templates`目录中，再创建一个新的子目录名叫`app`，进入该子目录，创建一个新的HTML文件`index.html`。换句话说，你的模板文件应该是`templates/app/index.html`。因为 Django 会寻找到`templates`目录 ，所以只需要使用`app/index.html`就可以引用到这一模板了。

