> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [docs.djangoproject.com](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial01/)

> 适合有截止日期的完美主义者的网络框架。

让我们通过示例来学习。

通过本教程，我们将引导您创建一个基本的投票应用程序。

该遗嘱由两部分组成：

*   一个让人们查看和投票的公共站点。
*   修改一个让你可以添加、和删除投票的管理站点。

我们假设你已经阅读了[安装 Django](https://docs.djangoproject.com/zh-hans/5.0/intro/install/)。你可以知道 Django 已经被安装，且安装的是哪个版本，通过在命令提示行输入命令（由 $ 远端）。

/ 

```
$ python -m django --version
```

```
...\> py -m django --version
```

如果这行命令输出了一个版本号，则证明你已经安装了此版本的 Django；如果你得到的是一个“No module named django”的错误提示，则表明你已经安装。

本教程是为 Django 5.0 编写的，它支持 Python 3.10 及更高版本。如果 Django 版本不匹配，您可以使用本页右下角的版本切换器参考您的 Django 版本的教程，或者将 Django 更新到最新版本。如果您使用的是较旧版本的 Python，请检查[我应该使用哪个版本的 Python 来配合 Django？](https://docs.djangoproject.com/zh-hans/5.0/faq/install/#faq-python-version-support)找到 Django 的兼容版本。

您可以查看文档[如何安装 Django](https://docs.djangoproject.com/zh-hans/5.0/topics/install/)来获得有关删除旧版本、安装新版本的流程和建议。

从哪里获得帮助：

如果您在阅读本教程的过程中有任何疑问，可以访问常见问题解答并[获取帮助](https://docs.djangoproject.com/zh-hans/5.0/faq/help/)的版本块。

创建项目[¶](#creating-a-project "永久链接至标题")
--------------------------------------

如果这是你第一次使用 Django 的话，你需要一些初始化设置。那么，你需要用一些自动生成的代码配置一个 Django[项目](https://docs.djangoproject.com/zh-hans/5.0/glossary/#term-project)——即一个 Django 项目实例需要的设置项集合，包括数据库配置、 Django 配置和应用程序配置。

打开命令行，`cd`到一个你想要放置你代码的目录，然后运行以下命令：

/ 

```
$ django-admin startproject mysite
```

```
...\> django-admin startproject mysite
```

这行代码将在当前目录下创建一个`mysite`目录。如果命令失败了，查看[运行 django-admin 时遇到的问题](https://docs.djangoproject.com/zh-hans/5.0/faq/troubleshooting/#troubleshooting-django-admin)，可能会给你提供帮助。

备注

你得避免使用 Python 或 Django 的内部保留字来命名你的项目。具体来说，你得避免使用像`django`（会和 Django 自己产生冲突）或 `test`（会和 Python 的内置组件产生冲突）这样的名字。

我的代码该放在哪？

如果你的背景是普通的现代复古 PHP（没有使用过框架），你可能习惯于把代码放在网络服务器的文档根目录下（比如`/var/www`）。在 Django 中，你不需要这样做。把任何 Python 代码放在网络服务器的文档根目录下都不是一个好主意，因为这可能使人们能够通过网络查看你的代码。这对安全没有好处。

把你的代码放在文档根目录**以外**的某些地方吧，比如 /home/mycode。

让我们看看[`startproject`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-startproject)制作了物品：

```
我的网站/
    管理.py
    我的网站/
        __init__.py
        设置.py
        urls.py
        阿斯吉
        wsgi.py
```

这些目录和文件的用处是：

*   最外层的`mysite/`根目录只是你项目的容器，根目录名称对 Django 没有影响，你可以将它重命名为任何你喜欢的名称。
*   `manage.py`：一个让你用各种方式管理 Django 项目的命令行工具。你可以阅读[django-admin 和 manage.py](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/)获取所有`manage.py`细节。
*   里面一层的`mysite/` 目录包含你的项目，它是一个纯Python包。它的名字就是当你引用它内部任何东西时需要用到的Python包名。（比如`mysite.urls`）。
*   `mysite/__init__.py`：一个空文件，告诉Python这个目录应该被认为是一个Python包。如果你是Python初学者，请阅读官方文档中的[更多关于包的知识](https://docs.python.org/3/tutorial/modules.html#tut-packages "（在 Python v3.12 中）")。
*   `mysite/settings.py`：Django 项目的配置文件。如果你想知道这个文件是如何工作的，请查看[Django 配置](https://docs.djangoproject.com/zh-hans/5.0/topics/settings/)了解细节。
*   `mysite/urls.py`：Django 项目的 URL 声明，就像你网站的“目录”。阅读[URL 调度器](https://docs.djangoproject.com/zh-hans/5.0/topics/http/urls/)文档来获取更多关于 URL 的内容。
*   `mysite/asgi.py`：作为您的项目在 ASGI 兼容的 Web 服务器上的入口运行。阅读[如何使用 ASGI 来部署](https://docs.djangoproject.com/zh-hans/5.0/howto/deployment/asgi/)了解更多细节。
*   `mysite/wsgi.py`：作为您的项目在 WSGI 兼容的 Web 服务器上的入口运行。阅读[如何使用 WSGI 进行部署](https://docs.djangoproject.com/zh-hans/5.0/howto/deployment/wsgi/)了解更多细节。

用于开发简单的服务器[¶](#the-development-server "永久链接至标题")
------------------------------------------------

让我们来确认一下你的Django项目是否真的创建成功了。如果你的当前目录不是外层的`mysite`目录的话，请切换到这个目录，然后运行下面的命令：

/ 

```
$ python 管理.py runserver
```

```
...\> py 管理.py  runserver
```

你应该会看到如下输出：

```
正在执行 系统检查...

系统 检查未发现任何问题（0 已静音）。

您 有未应用的迁移；在应用它们之前，您的应用程序可能无法正常工作。
运行 “python manage.py migrate”来应用它们。

2024 年二月 15 日 - 15:50:53 
Django 版本 5.0，使用设置 'mysite.settings'
在 http://127.0.0.1:8000/启动开发服务器 使用 CONTROL-C 退出服务器。
 
```

备注

忽略有关未应用最新数据库迁移的警告，稍后我们处理数据库。

你已经启动了 Django 开发服务器，这是一个用纯 Python 编写的轻量级网络服务器。我们在 Django 中包含了这个服务器，所以你可以快速开发，而不需要处理配置生产服务器的问题 -- 比如 Apache -- 直到你准备好进行生产。

现在是个提醒你的好时机：**千万不要**将此服务器用于和生产环境相关的任何地方。这个服务器只是为了开发和设计的。（我们在网络框架方面是专家，在网络服务器方面不是。）

服务器现在正在运行，通过浏览器访问[http://127.0.0.1:8000/](http://127.0.0.1:8000/)。您将看到一个“祝贺”页面，有一枚火箭正在发射。您成功了！

交换端口

默认情况下，[`runserver`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-runserver)命令保留服务器设置为监听本机内部 IP 的 8000 端口。

如果你想更换服务器的监听端口，请使用命令行参数。举个例子，下面的命令使服务器监听 8080 端口：

/ 

```
$ python管理.py运行服务器8080
```

```
...\> py 管理.py  runserver 8080
```

如果你想要修改服务器监听的IP，在端口之前输入新的。比如，要监听所有服务器的公开IP（这你运行Vagrant或想要向网络上的其他电脑展示你的结果时很有用），使用：

/ 

```
$ python manage.py runserver 0 . 0 . 0 . 0 ：8000
```

```
...\> py 管理.py  runserver 0 .0 .0 .0 :8000
```

关于这个简易服务器的完整信息可以在[`runserver`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-runserver)文档中找到。

会自动重新加载的服务器[`runserver`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-runserver)

用于开发的服务器在需要的情况下要每次的访问请求重新加载一遍Python代码。所以你不需要为了让修改的代码生效而关闭的重新启动服务器。但是，有一些动作，比如添加新文件，将不会触发自动重新加载，接下来你得自己手动重启服务器。

创建投票应用[¶](#creating-the-polls-app "永久链接至标题")
--------------------------------------------

现在你的开发环境——这个“项目”——已经配置好了，你可以开始干活了。

在 Django 中，每一个应用都是一个 Python 包，并且遵循着相同的约定。Django 自带一个工具，可以帮助生成应用的基础目录结构，这样你就可以专心写代码，而不是创建目录了。

项目VS应用

项目和应用有什么区别？应用是一个专门做某事的网络应用程序——比如博客系统，或者公共记录的数据库，或者小型的投票程序。项目封装一个网站使用的配置和应用的集合。项目可以包含很多个应用。应用可以被很多个项目使用。

你的应用可以安装在任何[Python路径](https://docs.python.org/3/tutorial/modules.html#tut-searchpath "（在 Python v3.12 中）")中定义的路径。在本教程中，我们将在你的`manage.py`同级目录下创建投票应用。这样它就可以作为顶级模块导入，而不是`mysite`子模块。

请确定您现在所在`manage.py`的目录下，然后运行该行命令来创建一个应用程序：

/ 

```
$ python manage.py startapp polls
```

```
...\> py 管理.py  startapp 民意调查
```

这将创建一个名为`polls`的目录，其布局如下：

```
民意调查/
    __init__.py
    管理员.py
    应用程序.py
    迁移/
        __init__.py
    模型.py
    测试.py
    视图.py
```

该目录结构包括投票应用的全部内容。

编写第一个视图[¶](#write-your-first-view "永久链接至标题")
--------------------------------------------

让我们开始编写第一个视图吧。打开`polls/views.py`，把下面这些Python代码输入进去：

`polls/views.py`[¶](#id1 "永久链接至代码")

```
从django.http导入HttpResponse


def  index (request) : 
    return HttpResponse( "Hello, world. You’re at the polls index." )
```

这是 Django 中最简单的视图。如果想看到效果，我们需要将一个 URL 映射到它上面——这就是我们需要 URLconf 的原因了。

要在 polls 目录中创建一个 URL 配置，请创建一个名为`urls.py`的文件。现在你的应用程序目录应该如下图所示：

```
民意调查/
    __init__.py
    管理员.py
    应用程序.py
    迁移/
        __init__.py
    模型.py
    测试.py
    urls.py
    视图.py
```

在`polls/urls.py`中，输入以下代码：

`polls/urls.py`[¶](#id2 "永久链接至代码")

```
从django.urls导入路径

从。导入视图

url 模式 = [
    路径（“”，views.index，名称= “索引”），
]
```

下一步是要在根 URLconf 文件中指定我们创建的`polls.urls`模块。在`mysite/urls.py`文件的`urlpatterns`列表里插入一个`include()`，如下：

`mysite/urls.py`[¶](#id3 "永久链接至代码")

```
从django.contrib导入管理
从django.urls导入包含，路径

url 模式 = [
    路径（“民意调查/”，包括（“民意调查.urls”）），
    路径（“admin/”，admin.site.urls），
]
```

函数[`include()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.include "django.urls.include")允许引用其他 URLconfs。每当 Django 遇到[`include()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.include "django.urls.include")时，它会截断与查找匹配的 URL 的部分，并将剩余的字符串发送到 URLconf 以供进一步处理。

我们设计[`include()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.include "django.urls.include")的理念是因为创建可以即插即用。投票应用有它自己的 URLconf( `polls/urls.py`)，它们能够被放在 "/polls/" ， "/fun_polls/" ，"/content/polls/" ，或者其他任何路径下，该应用都能够正常工作。

何时使用[`include()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.include "django.urls.include")

当包含其他 URL 模式时，您应该始终使用`include()`，`admin.site.urls`但例外。

你现在把`index`View添加进了URLconf。通过以下命令验证是否正常工作：

/ 

```
$ python 管理.py runserver
```

```
...\> py 管理.py  runserver
```

使用您的浏览器访问[http://localhost:8000/polls/](http://localhost:8000/polls/)，您应该能够看到“ _Hello, world. You’re at the polls index._ ”，这是您在`index`视图中定义的。

没有找到页面？

如果您在这里遇到一个错误页面，请检查一下您是否正访问 http://localhost:8000/polls/ 而不是 http://localhost:8000/。

具有四个参数的函数[`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path")，两个必须参数：`route`和`view`，两个可选参数：`kwargs`和`name`。现在，是时候来研究这些参数的意义了。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path")参数：`route`[¶](#path-argument-route "永久链接至标题")

`route`是一个匹配 URL 的准则（类似正则表达式）。当 Django 响应一个请求时，它会从`urlpatterns`的第一项开始，按顺序依次匹配列表中的项，直到找到匹配的项。

这些准则不会匹配 GET 和 POST 参数或域名。例如，URLconf 在处理请求`https://www.example.com/myapp/`时，它会尝试匹配`myapp/`。处理请求`https://www.example.com/myapp/?page=3`时，也只会尝试匹配`myapp/`。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path")参数：`view`[¶](#path-argument-view "永久链接至标题")

当 Django 找到了一个匹配的准则时，就会调用这个特定的视图函数，并确定一个[`HttpRequest`](https://docs.djangoproject.com/zh-hans/5.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest")对象作为第一个参数，被“捕获”的参数以关键字参数的形式表示。稍后，我们会出一个例子。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path")参数：`kwargs`[¶](#path-argument-kwargs "永久链接至标题")

任何一个关键字参数都可以作为一个字典传递给目标视图函数。本教程中不会使用此功能。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path")参数：`name`[¶](#path-argument-name "永久链接至标题")

为你的 URL 取名使你能够在 Django 的任何地方唯一地引用它，尤其是在模板中。这个有用的功能允许你只修改一个文件就可以全局修改某个 URL 模式。

当您了解了基本的请求和响应流程后，请阅读[教程的第 2 部分](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial02/) 开始使用数据库。