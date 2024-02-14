> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [harminder.dev](https://harminder.dev/projects/websites/mkdocs/knowledge-base/adding-assets/additional-css/tailwind-css/setup-material-mkdocs-insiders-tailwind/#build-mkdocs)

> 将 Tailwind CSS 的强大功能与 Material for MkDocs Insiders 相结合，创建一个文档网站......

介绍
--

将 Tailwind CSS 的强大功能与 Material for MkDocs Insiders 相结合，创建一个文档站点，该站点不仅具有 Material 的强大功能和美观的设计，而且还具有 Tailwind CSS 的实用性优先的灵活性和自定义功能。本指南将引导您完成将 Tailwind CSS 集成到 MkDocs 项目中的步骤，让您轻松定制站点的外观和风格。

为 MkDocs 设置材料
-------------

首先根据官方指南为 MkDocs 设置材质。在继续集成 Tailwind CSS 之前，请确保您有正在运行的 MkDocs 环境。

安装 Tailwind CSS
---------------

在 MkDocs 项目目录中，运行以下命令来安装必要的包：

```
npm 安装 tailwindcss postcss autoprefixer


```

这些软件包包括用于实用程序优先 CSS 框架的 Tailwind CSS、用于使用 JavaScript 处理 CSS 的 PostCSS 以及用于处理 CSS 供应商前缀的 Autoprefixer。

设置 PostCSS 和 Tailwind
---------------------

在 MkDocs 项目的根目录中创建一个`postcss.config.js`包含以下内容的文件：

```
模块. 导出 = {
  插件：[require('tailwindcss'), require('autoprefixer')]
}


```

然后，初始化 Tailwind 配置文件，Tailwind 使用该文件读取自定义设置：

创建您的自定义 CSS 文件
--------------

`docs/stylesheets/`在名为 的目录中创建一个新的 CSS 文件`tailwind.css`。此文件将导入 Tailwind 的图层供您在整个项目中使用：

```
@import 'tailwindcss/base';
@import 'tailwindcss/组件';
@import 'tailwindcss/utilities';


```

构建 Tailwind CSS
---------------

通过运行以下命令，使用 Tailwind 的样式编译 CSS：

```
npx tailwindcss 构建文档/样式表/tailwind.css -o 文档/样式表/output.css


```

此命令将处理您的自定义 CSS 文件并输出包含所有 Tailwind 实用程序类的完全编译的 CSS 文件。

更新 MkDocs 配置
------------

通过编辑以下内容将编译后的 CSS 包含在您的 MkDocs 配置中`mkdocs.yml`：

```
额外的CSS：
  - 样式表/output.css


```

定制化
---

现在，您可以在 Markdown 内容或 MkDocs 站点中使用的任何 HTML 模板中使用 Tailwind CSS 类。

构建 MkDocs
---------

要查看更改生效，请使用以下命令构建 MkDocs 项目：

```
mkdocs 构建


```

注意：对 Tailwind 或自定义 CSS 进行的任何更改都需要您重建 Tailwind CSS，然后重建您的 MkDocs 项目。