> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/lxd_max/article/details/131766544)

`requirements.txt`文件是一种用于指定 Python 项目依赖项的简单方法。它是一个包含要安装的 Python 包及其版本的纯文本文件。

Poetry 是一个 Python 包管理和依赖解析工具，用于简化 Python 项目的依赖管理。

**目录**

[requirements 使用](#t0)

[Poetry 使用](#t1)

[优势](#t2)

### requirements 使用

**`requirements.txt是`**包含要安装的 Python 包及其版本的纯文本文件。

使用步骤：  
（1）创建**`requirements.txt`**`文件，`列出项目所需的所有 Python 包及其版本，比如：

```
Django==3.2.4
celery==4.4.0
Django==3.2.4
celery==4.4.0
```

（2）安装`requirements.txt`文件中列出的依赖项（如果速度太慢，还可以用 -i 参数跟镜像加速）

```
pip install -r requirements.txt

```

（3）更新：手动编辑即可，然后重新 pip install -r requirements.txt。

（4）卸载：

```
pip uninstall -r requirements.txt

```

（5）已在虚拟环境中安装依赖项，但尚未创建`requirements.txt`文件，可以使用以下命令自动生成一个包含所有包及其版本的`requirements.txt`文件：

```
pip freeze > requirements.txt

```

### Poetry 使用

（1）安装 Poetry，比如安装 Poetry version 1.1.15：

```
pip install Poetry==1.1.15

```

（2）使用 Poetry 创建一个新的 Python 项目

```
poetry new my_project

```

![](https://img-blog.csdnimg.cn/c4148c1b29114138ac7e5d320dde85b6.png)

（3）导航并初始化 Poetry

```
cd my_project
poetry init
```

此时的 pyproject.toml 文件内容

![](https://img-blog.csdnimg.cn/7efa0b8cf9234fc59b692b64b7b624e5.png)

（4）**添加依赖项**，比如添加一个 django 2.2.28 版本，

```
poetry add django==2.2.28

```

此时的 pyproject.toml 文件内容

![](https://img-blog.csdnimg.cn/80515bb170324f789ad7a154173807fc.png)

同时还自动创建或更新 poetry.lock 文件以确保项目的一致性和可重现性，在添加依赖性后项目的所有依赖都会更新。

 ![](https://img-blog.csdnimg.cn/1bc5dab6c5fc47f6884b037d59922675.png)

 （5）如果手动去修改 pyproject.toml，可以用以下命令更新依赖项

```
poetry update

```

（6）**运行项目**：要在 Poetry 的虚拟环境中运行项目，确保脚本在 Poetry 管理的虚拟环境中运行

```
poetry run python <your_script.py>

```

比如创建 hello.py 文件，运行

```
poetry run python my_project/hello.py

```

![](https://img-blog.csdnimg.cn/39555d5b463346659a37d3daf686f35a.png)

![](https://img-blog.csdnimg.cn/0f19fdb559484c658f26b19b5faa7df6.png)

 （7）**构建和发布包**：要构建并发布您的 Python 包，将自动生成所需的`setup.py`、`setup.cfg`和`MANIFEST.in`等文件，并将包发布到 PyPI。

```
poetry build
poetry publish
```

### 优势

**Poetry 优势**：

1.  依赖解析：Poetry 可自动解析项目的依赖关系，确保安装的包版本间没有冲突。这与`requirements.txt`不同，后者需手动管理和指定每个包的版本。
2.  锁定文件：使用`poetry.lock`的锁定文件确保项目在不同环境中的一致性和可重现性。这意味着在其他环境中安装依赖项时，Poetry 会确保使用与锁定文件中记录的相同版本。
3.  环境隔离：Poetry 支持自动创建和管理虚拟环境，从而确保项目的依赖项与系统全局安装的包隔离。这有助于避免因全局包版本冲突而导致的问题。
4.  包发布：Poetry 提供了一种简单的方法来构建和发布 Python 包。它可以自动生成`setup.py`、`setup.cfg`和`MANIFEST.in`等文件，从而简化了发布过程。
5.  简化的命令行界面：Poetry 提供了一个简洁、统一的命令行界面，用于管理项目的依赖关系、虚拟环境和包发布。与`requirements.txt`相比，这使得依赖管理更加直观和一致。

**requirements.txt 优势**：

1.  简单易用：简单直接指定项目依赖项。不需要额外的工具或配置，只需纯文本文件。
2.  广泛支持：许多第三方工具和服务都支持它。有助于 Python 生态系统的兼容性。
3.  更低的学习曲线：对于已经习惯使用`requirements.txt`和`pip`的开发人员来说，这种方法可能更加简单和熟悉。

日常开发中，选择 Poetry 或`requirements.txt`取决于项目需求和个人喜好：

*   如果需要更高级的依赖管理功能（如自动依赖解析、环境隔离和包发布）或项目较为复杂，并且愿意学习和适应新工具，那么 Poetry 可能是一个更好的选择。
*   如果您的项目相对简单，或者您已经习惯使用`requirements.txt`和`pip`，那么继续使用`requirements.txt`可能更加方便和实用。