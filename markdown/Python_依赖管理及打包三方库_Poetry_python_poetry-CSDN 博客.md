> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/wohu1104/article/details/127769839)

1. 简介
-----

`Poetry` 是 `Python` 中用于依赖管理和打包的工具。它允许您声明项目所依赖的库，并将为您管理（安装 / 更新）它们。`Poetry` 提供了一个锁定文件以确保可重复安装，并且可以构建您的项目以进行分发。

`Poetry` 将所有的配置都放置在一个 `toml` 文件（`pyproject.toml`）中，这些配置包括：依赖管理、构建、打包、发布。

作为一个传统虚拟环境的实现，`Poetry` 凭借其强大的依赖分析能力被大量项目所推荐的虚拟环境管理工具。

2. 安装卸载
-------

### 2.1 安装（官方建议）

默认安装

```
curl -sSL https://install.python-poetry.org | python3 -

```

环境变量：

*   `POETRY_HOME`：安装目录
*   `POETRY_PREVIEW`：是否安装预发布版本，如果需要，设置为 POETRY_PREVIEW=1
*   `POETRY_VERSION`：指定安装的版本
*   `--git`：从 `git`存储库中安装

安装示例

```
curl -sSL https://install.python-poetry.org | python3 - --git https://github.com/python-poetry/poetry.git@master
curl -sSL https://install.python-poetry.org | POETRY_VERSION=1.2.0 python3 -


```

### 2.2 配置环境变量

`poetry`安装程序在一个众所周知的、特定于平台的目录中创建一个包装器：

*   `$HOME/.local/bin`在 `Unix` 上
*   `%APPDATA%\Python\Scripts`在 `Windows` 上
*   `$POETRY_HOME/bin`如果 `$POETRY_HOME`设置

### 2.3 其它方式安装

```
pip install poetry

```

`pip`这种安装方式可能引起依赖包冲突。还可以考虑使用 `pipx`（3.6 及之后的版本）或者 `pipsi`（3.6 之前的版本）

安装后可以使用下面的命令确认安装成功：

```
poetry --version

```

### 2.4 更新 poetry

```
poetry self update  					# 更新到最新版本
poetry self update --preview  # 更新到预览版本
poetry self update 1.2.0  		# 更新到指定的版本


```

**注意：**  
`self update`命令仅在推荐安装程序安装 `Poetry`时有效。

### 2.5 卸载 poetry

```
curl -sSL https://install.python-poetry.org | python3 - --uninstall
curl -sSL https://install.python-poetry.org | POETRY_UNINSTALL=1 python3 -


```

3. 项目使用
-------

### 3.1 创建项目

```
poetry new project_demo

```

新建项目的目录结构如下：

```
$ tree project_demo/
project_demo/
├── project_demo
│   └── __init__.py
├── pyproject.toml
├── README.md
└── tests
    └── __init__.py

2 directories, 4 files

```

同时，项目下会自动生成 `pyproject.toml`文件，该文件的主要用途是依赖管理、构建、打包、发布，内容如下：

```
$ cat pyproject.toml 
[tool.poetry]
name = "project-demo"
version = "0.1.0"
description = ""
authors = ["xxxx@163.com <xxxxx@163.com>"]
readme = "README.md"
packages = [{include = "project_demo"}]

[tool.poetry.dependencies]
python = "^3.8"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"


```

### 3.2 依赖管理

如果要向项目添加依赖项，可以在 `tool.poetry.dependencies`部分中指定它们。

```
[tool.poetry.dependencies]
pendulum = "^2.1"
loguru = "^0.6.0"

```

它采用**包名称**和**版本约束**的映射。

在这里您可以列出项目需要的所有依赖包。 就像旧 `requirements.txt` 文件一样。 您可以手动完成此操作，然后调用命令 `poetry install` 以将其全部安装以用于软件包开发和工作目的。

您可以使用 `add`命令而不是手动修改文件。如果您使用 `poetry add <dependency_name>`安装依赖包 相当于使用`pip install <dependency_name>` 。

我们注意到这里指定了 `python = "^3.8"`。这是由创建工程时，运行 `poetry`的 `python`版本来决定的。

### 3.3 修改安装包源

在 `pyproject.toml`文件末尾添加

```
[[tool.poetry.source]]
name = "aliyun"
url = "https://mirrors.aliyun.com/pypi/simple"
default = true

```

用于指定安装三方库时的源。当配置的源中没有指定的包时会到 PyPI 中搜索。

使用这种方式时，有注意一个情况，就是在第一次运行后，会生成一个 `poetry.lock`的文件，将第三方库的版本锁定，这意味着，后面的开发人员根据 `pyproject.toml`安装的时候，第三方库的版本是固定的。

当 `poetry.lock`文件存在时运行会解析并安装其中列出的所有依赖项 `pyproject.toml`， `Poetry` 使用中列出的确切版本 `poetry.lock`来确保包版本对于在您的项目中工作的每个人都是一致的。

`poetry.lock`文件会阻止您自动获取最新版本的依赖项。要更新到最新版本，请使用 `update`命令。这将获取最新的匹配版本并使用新版本更新锁定文件。（这相当于删除 `poetry.lock`文件并 `install`重新运行。）

### 3.4 **已有项目初始化**

`Poetry` 可用于 “初始化” 预填充目录，而不是创建新项目。在目录中以交互方式创建 `pyproject.toml`文件 `pre-existing-project`

```
cd pre-existing-project
poetry init


```

根据它的提示输入你的项目信息，不确定的内容就按下 `Enter` 使用默认值，后续也可以手动更新。

### 3.5 运行项目

要运行您的脚本，只需使用

```
poetry run python main.py

```

每次在虚拟环境下做点啥事，命令前面都要加上 `poetry run`，有点太麻烦了。  
这时可以使用下面这条命令，直接激活当前的虚拟环境同时：

```
poetry shell  # 进入
exit  # 退出

```

### 3.6 列出可用包

要列出所有可用的包，可用下面命令：

```
poetry show


```

如果要查看某个包的详细信息，可以传递包名。

```
$ poetry show loguru
 name         : loguru                                
 version      : 0.6.0                                 
 description  : Python logging made (stupidly) simple 

dependencies
 - colorama >=0.3.4
 - win32-setctime >=1.0.0


```

4. 虚拟环境管理
---------

### 4.1 **创建虚拟环境**

#### 4.1.1 方式一

当参数 `**virtualenvs.create=true**` (默认) 时，执行 `**poetry install/add/remove**` 时会检测当前项目是否有虚拟环境，没有就自动创建（确保当前目录有 `pyproject.toml` 文件）。

这个命令会读取 `pyproject.toml` 中的所有依赖（包括开发依赖）并安装，如果不想安装开发依赖，可以附加 `--no-dev` 选项。如果项目根目录有 `poetry.lock` 文件，会安装这个文件中列出的锁定版本的依赖。

#### 4.1.2 **方式二**

指定创建虚拟环境时使用的 `Python` 解释器版本，如下：

```
poetry env use python3

```

`python3`是 `python`解释器，相当于 `cmd`下输入 `python3`。

还可以指定解释器路径来创建：

```
poetry env use /usr/bin/python


poetry env use /home/wohu/.miniconda3/envs/py37/bin/python

```

### 4.2 **管理虚拟环境**

执行 poetry 开头的命令并不需要激活虚拟环境，因为它会自动检测到当前虚拟环境。如果你想快速在当前目录对应的虚拟环境中执行命令，可以使用 `poetry run`

```
poetry run python main.py

```

如果你想显式的激活虚拟环境，使用 `poetry shell` 命令：

```
poetry shell

```

其它相关命令

```
poetry env info								# 查看虚拟环境信息
poetry env list								# 显示虚拟环境所有列表
poetry env list --full-path		# 显示虚拟环境绝对路径


```

### 4.3 删除虚拟环境

1.  可以直接删除虚拟环境文件夹
2.  通过 poetry env remove 命令删除

```
# 执行删除虚拟环境时，需要指定对应的解析器版本
poetry env remove python3

# 移除指定的环境
poetry env remove /full/path/to/python
poetry env remove python3.7
poetry env remove 3.7
poetry env remove test-O3eWbxRl-py3.7

poetry env remove --all  # 移除所有的环境


```

### 4.4 **查看 python 版本**

```
poetry run python -V

```

5. **依赖包管理**
------------

### 5.1 **安装依赖包**

可以使用 `install`命令直接解析并安装 `pyproject.toml`的依赖包

```
poetry install

```

也可以可以使用 `add`命令来安装 `Python`工具包，

```
poetry add numpy

```

还可以，通过添加配置参数 `--dev`来区分不同环境下的依赖包。

```
poetry add pytest --dev

```

`poetry add`依赖安装常见命令如下表格：  
![](https://img-blog.csdnimg.cn/b693485fee31463d83c353ea42506202.png#pic_center)

### 5.2 更新、卸载、查看依赖包

```
poetry update					# 更新所有锁定版本的依赖包
poetry update numpy		# 更新指定依赖包
poetry remove numpy		# 卸载依赖包

poetry show   					# 查看项目安装的依赖
poetry show --outdated  # 查看可以更新的依赖
poetry show --tree  		# 以树形结构查看项目安装的依赖关系

```

6. 环境配置
-------

### 6.1 配置参数

`Poetry`提供了全局配置 `config.toml`和特定项目的配置 `poetry.toml`。

```
poetry config virtualenvs.create true								# 全局配置
poetry config virtualenvs.create true --local				# 特定项目配置
poetry config virtualenvs.create --unset						# 重置全局配置
poetry config virtualenvs.create --local --unset 		# 重置项目配置

```

列出当前项目的配置

```
poetry config --list

```

```
$ poetry config --list
cache-dir = "/home/wohu/.cache/pypoetry"
experimental.new-installer = true
experimental.system-git-client = false
installer.max-workers = null
installer.no-binary = null
installer.parallel = true
repositories.aliyun.url = "https://mirrors.aliyun.com/pypi/simple"
virtualenvs.create = true
virtualenvs.in-project = null
virtualenvs.options.always-copy = false
virtualenvs.options.no-pip = false
virtualenvs.options.no-setuptools = false
virtualenvs.options.system-site-packages = false
virtualenvs.path = "{cache-dir}/virtualenvs"  # /home/wohu/.cache/pypoetry/virtualenvs
virtualenvs.prefer-active-python = false
virtualenvs.prompt = "{project_name}-py{python_version}"

```

`Poetry`支持的常用参数有：  
![](https://img-blog.csdnimg.cn/5cc0f3a28c404c33811e5fe1695a1e6e.png#pic_center)

### 6.2 相关操作

```
# Poetry 提供了通过将--local选项传递给config命令来获得特定于项目的设置的能力
poetry config virtualenvs.create false --local

# 列出当前的配置
poetry config --list

# 显示特定配置的值
poetry config virtualenvs.path

# 修改特定配置的值
poetry config virtualenvs.path D:\\poetryCache

# 移除某一配置默认的值
poetry config virtualenvs.path --unset


```

7. **常用命令汇总**
-------------

`poetry` 提供了一系列覆盖整个开发流程的命令，这些命令使用简单，如表所示：  
![](https://img-blog.csdnimg.cn/0025ac0a6ac84e30bebd10e1515a1819.png#pic_center)  
导出 requirements.txt 文件

```
poetry export --without-hashes  --output requirements.txt

```

8. 参考文档
-------

[https://python-poetry.org/docs/](https://python-poetry.org/docs/)  
[https://zhuanlan.zhihu.com/p/445952026](https://zhuanlan.zhihu.com/p/445952026)  
[https://www.kancloud.cn/madxzb/python-guide/2248089](https://www.kancloud.cn/madxzb/python-guide/2248089)  
[https://blog.csdn.net/qq_62789540/article/details/127353963](https://blog.csdn.net/qq_62789540/article/details/127353963)