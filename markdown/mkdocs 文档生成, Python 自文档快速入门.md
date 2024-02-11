> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/BEMAKE/p/mkdocsTutorials.html)

mkdocs 文档生成
===========

目录

*   [mkdocs 文档生成](#mkdocs-文档生成)
    *   [说明](#说明)
    *   [安装](#安装)
    *   [书写代码](#书写代码)
    *   [mkdocs 部分](#mkdocs-部分)
        *   [创建 docs](#创建-docs)
        *   [配置 mkdocs.yml](#配置-mkdocsyml)
        *   [生成文档](#生成文档)
        *   [导航栏的设置](#导航栏的设置)
    *   [美化以及更多设置](#美化以及更多设置)
        *   [改变主题颜色](#改变主题颜色)
        *   [改变明暗主题](#改变明暗主题)
        *   [更改语言](#更改语言)
        *   [设置 logo 和图标](#设置-logo-和图标)
    *   [更多设置一键到位](#更多设置一键到位)

说明[#](#说明)
----------

在 Python 中写文档是一件很累的事情，有的时候写文档比写代码还要累，于是这个时候就有一个自动写文档的工具，就是 mkdocs，通过自动提取文章中的谷歌函数注释来达到对模块中的功能进行自动写文档的效果

而大部分的文档都有点复古，直到我看到了 pydantic 的文档，我去了解一下才发现，这个是用 mkdocs 写出来的，于是就有了这一篇教程

[Pydantic 文档](https://docs.pydantic.dev/latest/)

mkdocs 可以从代码中提取函数文档，模块文档，以及类文档，将他们按照你的要求展示出来，同时他还有很多扩展插件可以显示 mermaid 流程图，数据公式，另外这个 mkdocs 也有人拿来写博客，不过这里就不多提及

安装[#](#安装)
----------

在开始写文档之前，需要安装 3 个库

*   mkdoc
    *   主要的文档库核心
*   mkdocstrings[python]
    *   负责从代码中提取文档
*   mkdocs-material
    *   一个好看的代码主题

```
pip install mkdocs -U
pip install "mkdocstrings[python]" -U
pip install mkdocs-material -U


```

书写代码[#](#书写代码)
--------------

现在我们需要一些已经写好的代码，现在把他们放到 src 文件夹下面

```
- src
	- calc.py


```

```
"""This module provides some basic math functions.

该程序提供了一些基本的数学函数，包括加减乘除四则运算。

Example:
    >>> add(1, 2)
    3.0
    >>> subtract(1, 2)
    -1.0
    >>> multiply(1, 2)
    2.0
    >>> divide(1, 2)
    0.5
"""

from typing import TypeVar

T = TypeVar('T', int, float)


def add(a: T, b: T) -> float:
    """Add two numbers.

    Args:
        a (T): 第一个数
        b (T): 第二个数

    Returns:
        float: 两个数的和
    """
    return float(a + b)


def subtract(a: T, b: T) -> float:
    """Subtract two numbers.

    Args:
        a (T): 第一个数
        b (T): 第二个数

    Returns:
        float: 两个数的差
    """
    return float(a - b)


def multiply(a: T, b: T) -> float:
    """Multiply two numbers.

    Args:
        a (T): 第一个数
        b (T): 第二个数

    Returns:
        float: 两个数的积
    """
    return float(a * b)


def divide(a: T, b: T) -> float:
    """Divide two numbers.

    Args:
        a (T): 第一个数
        b (T): 第二个数

    Raises:
        ZeroDivisionError: 除数为0

    Returns:
        float: 两个数的商
    """
    if b == 0:
        raise ZeroDivisionError("division by zero")
    return float(a / b)



```

mkdocs 部分[#](#mkdocs-部分)
------------------------

### 创建 docs[#](#创建-docs)

在工程目录下 (src 的父文件夹), 输入下面的代码

```
mkdocs new ./


```

然后等待创建成功，如果创建成功可以看到目录里面多了一个 `mkdocs.yml` 文件和`docs`文件夹紧接着我们要配置这个文件

### 配置 mkdocs.yml[#](#配置-mkdocsyml)

这个文件默认长成这个样子

```
site_name: My Docs


```

现在我们要对他进行配置，默认都可以直接写成下面这个样子

```
site_name: My Docs

theme:
  name: "material"

plugins:
- mkdocstrings:
    handlers:
      python:
        paths: [src]


```

### 生成文档[#](#生成文档)

紧接着我们可以通过下面的命令生成文档

```
mkdocs build


```

如果之前的文档后后来的文档有较大的差别，也可以使用下面的命令，下面的命令会删除那些多余的文件

```
mkdocs build --clean


```

然后我们就能看到现在多了一个 site 文件夹，这个文件夹里面就是已经生成好的网站，现在我们有两种方法可以查看， 一种是直接打开，还有一种可以通过在命令行里面输入

```
mkdocs serve


```

来打开一个动态的服务器，当文档内容发生改变的时候页面也会实时刷新，**推荐使用这种方法**

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203835825-1702618934.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203835825-1702618934.png)

虽然我们现在成功的打开了网页，但是我们发现现在页面里面什么都没有，我们接下来要去配置一下 index.md

在 index.md 里面，新建一行，里面通过三个冒号后面跟上 Python 模块的方式来实现读取指定的 Python 模块

```
# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

## 文章内容

::: src.calc



```

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203849753-1980412488.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203849753-1980412488.png)

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203858257-603149511.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203858257-603149511.png)

可以看到现在就生成了新的文档注释

### 导航栏的设置[#](#导航栏的设置)

有的时候我们还会需要导航栏的设置，这个时候可以通过在 docs 下面新建 `.md` 文件实现导航的效果，Mkdocs 会把每一个 Markdown 文件变成一个单独的页面，默认情况下文件按照字母的顺序进行输出，但是如果想要手动配置也可以通过在 mkdocs.yml 里面定义 nav 来实现

nav 的前面是 key，同时也是显示在页面内的名字，而后面的 `.md` 则是文件的路径，表示这个名字对应的是哪一个 `.md` 文件

```
nav:
  - Home: index.md
  - 教程: tutorials.md
  - guides: how-to-guides.md
  - 更多:
    - 更多/中文.md


```

其中有一个地方需要注意的就是，如果你的文件保存在文件夹里面，比如这里面是 `docs/更多` 这个文件夹，那么之后下这个文件夹里面的文件都需要使用相对路径，比如这个地方里面就是 `更多/中文.md`

美化以及更多设置[#](#美化以及更多设置)
----------------------

### 改变主题颜色[#](#改变主题颜色)

默认的主题是 indigo(蓝色)，不过也可以通过简单的命令来让主题发生改变现在的主题有下面的这些颜色可以选择

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203907113-1761227531.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203907113-1761227531.png)

主题的颜色同时也有两种需要设置

*   primary 主要颜色
    *   用于标题、侧边栏、文本链接和其他几个组件
*   accent 强调色
    *   强调色用于表示可以交互的元素，例如悬停的链接、按钮和滚动条

通常都是两个一起更改

```
theme:
  palette: 
    - primary: deep orange
    - accent: deep orange


```

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203914800-386754958.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203914800-386754958.png)

### 改变明暗主题[#](#改变明暗主题)

在 mkdocs 里面可以很轻松的改变主题的明暗，通过设置 mkdocs.yml 来实现

```
theme:
  name: "material"
  palette: 
    # Palette toggle for light mode
    - scheme: default
      toggle:
        icon: material/weather-night
        name: 切换到暗色模式

    # Palette toggle for dark mode
    - scheme: slate
      toggle:
        icon: material/weather-sunny
        name: 切换到亮色模式


```

然后现在就就能注意到我们的页面右上角多了一个图标

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203921760-1316721790.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203921760-1316721790.png)

我们点击一下就能切换到暗色模式，同时如果鼠标悬停在上面还能看到提示

[![](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203929156-1338015019.png)](https://img2023.cnblogs.com/blog/2794537/202312/2794537-20231215203929156-1338015019.png)

### 更改语言[#](#更改语言)

该选项被更改之后，search 选项也会被更改，如果作为一个代码文档我现在还没看到什么区别，好像只有右边的目录变成了中文，可能也有一些其他的项也变成了中文，我还没有具体的测试，不过现在可以这样设置

```
theme:
  language: zh


```

### 设置 logo 和图标[#](#设置-logo-和图标)

mkdocs 内置了很多图标，可以前往下面的链接搜索

> [https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/#using-icons](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/#using-icons)

其中以 material 开头的是能用的

```
theme:
  icon:
    logo: material/github


```

图标的设置则麻烦一点，没有办法用内置的

更多设置一键到位[#](#更多设置一键到位)
----------------------

如果你是一个技 术博客或者是使用 mkdocs 来做代码文档，那么直接复制我下面的设置然后改一改 extras 里面的作者地址和 nav 即可，代码请放置在 src 下面

```
theme:
    icon:
        logo: material/home
    name: material
    language: zh
    palette:
        # 切换到亮色
        - media: "(prefers-color-scheme: light)" # 根据系统的颜色模式自动切换
          scheme: default
          primary: pink
          accent: pink
          toggle:
              icon: material/weather-night
              name: 切换到暗色模式

        # 切换到暗色
        - media: "(prefers-color-scheme: dark)"
          scheme: slate
          primary: pink
          accent: pink
          toggle:
              icon: material/weather-sunny
              name: 切换到亮色模式
    features:
        - navigation.instant # 现在页面不会跳转,而是类似单页应用,搜索和各种跳转都是在当前页面完成,对美观有很大帮助
        - navigation.tabs # 页面上方的标签页
        - navigation.tracking # 页面滚动时，导航栏高亮当前页面
        - navigation.sections # 使导航栏分块
        - navigation.expand # 默认展开导航
        - navigation.prune # 只渲染当前页面的导航
        - toc.follow # 滚动的时候侧边栏自动跟随
        - navigation.top # 返回顶部按钮
        - search.suggest # 补全建议
        - search.highlight # 搜索结果高亮
        - search.share # 搜索结果分享
        - navigation.footer # 页脚提示下一章
        - content.code.copy # 代码段上的赋值按钮

markdown_extensions:
    - admonition # 警告语法
    - def_list
    - footnotes
    - abbr
    - pymdownx.caret
    - pymdownx.mark
    - pymdownx.tilde
    - md_in_html
    - pymdownx.arithmatex: # latex支持
          generic: true
    - toc:
          permalink: true # 固定标题位置为当前位置
          toc_depth: 3 # 目录深度
    - pymdownx.highlight: # 代码块高亮
          anchor_linenums: true
          linenums: true # 显示行号
          use_pygments: true # 代码高亮
          pygments_lang_class: true
          auto_title: true # 显示编程语言名称
          linenums_style: pymdownx-inline # 行号样式,防止复制的时候复制行号
    - pymdownx.betterem # 强调美化,比如**text**会被美化
    - pymdownx.caret # 上标和下标
    - pymdownx.mark # 上标和下标
    - pymdownx.tilde # 上标和下标
    - pymdownx.keys # 显示按键组合
    - pymdownx.critic
    - pymdownx.details # 可以折叠的代码块 ??? note 可以让警告变成折叠的
    - pymdownx.inlinehilite
    - pymdownx.snippets
    - pymdownx.superfences
    - pymdownx.magiclink # 自动识别链接
    - pymdownx.smartsymbols # 智能符号
    - pymdownx.snippets # 代码段
    - pymdownx.tasklist:
          custom_checkbox: true # 自定义复选框
    - attr_list
    - pymdownx.emoji:
          emoji_index: !!python/name:material.extensions.emoji.twemoji
          emoji_generator: !!python/name:material.extensions.emoji.to_svg
    - pymdownx.superfences: # 代码块中支持Mermaid
          custom_fences: # 支持 Mermaid
              - name: mermaid
                class: mermaid
                format: !!python/name:pymdownx.superfences.fence_code_format
    - pymdownx.tabbed:
          alternate_style: true
          combine_header_slug: true
    - pymdownx.tasklist:
          custom_checkbox: true
          clickable_checkbox: true
    - meta # 支持Markdown文件上方自定义标题标签等
    - tables

# 下面的是需要自定义的内容，请不要修改上方的内容，上面都是在开启各种插件和功能

site_name: 文档的名称 # 设置文档名称
copyright: Copyright © Python调包侠 # 左下角的版权声明

# nav:
#   - Home: index.md
#   - 教程: tutorials.md
#   - guides: how-to-guides.md
#   - 更多:
#       - 更多/中文.md
#   - 接口文档:
#       - interface/1.md

plugins:
    - mkdocstrings:
          handlers:
              python:
                  paths: [src]
    - search # 搜索插件
    # - offline # 离线本地搜索，和navigation.instant不能同时启用

extra:
    # generator: false  #删除页脚显示“使用 MkDocs 材料制造”
    social:
        - icon: fontawesome/brands/github
          link: https://github.com/271374667
          name: GitHub
        - icon: fontawesome/brands/bilibili
          link: https://space.bilibili.com/282527875
          name: Bilibili
        - icon: material/email
          link: Python调包侠:<271374667@qq.com>
          name: Email



```

更多内容请前往观看视频教程

[https://www.bilibili.com/video/BV12z4y1c7EN/](https://www.bilibili.com/video/BV12z4y1c7EN/)