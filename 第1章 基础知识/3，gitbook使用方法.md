# gitbook使用方法



GitBook 是使用 GitHub / Git 和 Markdown（或AsciiDoc）构建漂亮书籍的命令行工具（和Node.js库）。

GitBook 可以将您的内容作为网站（可定制和可扩展）或电子书（PDF，ePub或Mobi）输出。

[GitBook.com](https://link.zhihu.com/?target=https%3A//www.gitbook.com/) 是使用 GitBook 格式创建和托管图书的在线平台。它提供托管，协作功能和易于使用的编辑器。

# **GitBook 安装**

## 本地安装

### 环境要求

安装 GitBook 是很简单的。您的系统只需要满足这两个要求：

- NodeJS（推荐使用v4.0.0及以上版本）
- Windows，Linux，Unix 或 Mac OS X

## 通过NPM安装

安装 GitBook 的最好办法是通过 **NPM**。在终端提示符下，只需运行以下命令即可安装 GitBook：

```text
$ npm install gitbook-cli -g
```

`gitbook-cli` 是 GitBook 的一个命令行工具。它将自动安装所需版本的 GitBook 来构建一本书。

执行下面的命令，查看 GitBook 版本，以验证安装成功。

```text
$ gitbook -V
```

## 安装历史版本

`gitbook-cli` 可以轻松下载并安装其他版本的GitBook来测试您的书籍：

```text
$ gitbook fetch beta
```

使用 `gitbook ls-remote` 会列举可以下载的版本。

# 创建一本书

## 初始化

GitBook可以设置一个样板书：

```text
$ gitbook init
```

如果您希望将书籍创建到一个新目录中，可以通过运行 `gitbook init ./directory` 这样做。

## 构建

使用下面的命令，会在项目的目录下生成一个 `_book` 目录，里面的内容为静态站点的资源文件：

```text
$ gitbook build
```

## Debugging

您可以使用选项 `--log=debug` 和 `--debug` 来获取更好的错误消息（使用堆栈跟踪）。例如：

```text
$ gitbook build ./ --log=debug --debug
```

## 启动服务

使用下列命令会运行一个 web 服务, 通过 `http://localhost:4000/` 可以预览书籍

```text
$ gitbook serve
```

**GitBook 命令**

这里主要介绍一下 GitBook 的命令行工具 `gitbook-cli` 的一些命令, 首先说明两点:

- `gitbook-cli` 和 `gitbook` 是两个软件
- `gitbook-cli` 会将下载的 gitbook 的不同版本放到 `~/.gitbook`中, 可以通过设置`GITBOOK_DIR`环境变量来指定另外的文件夹

**列出 gitbook 所有的命令**

```text
gitbook help
```

**输出** **`gitbook-cli`** **的帮助信息**

```text
gitbook --help
```

**生成静态网页**

```text
gitbook build
```

**生成静态网页并运行服务器**

```text
gitbook serve
```

**生成时指定gitbook的版本, 本地没有会先下载**

```text
gitbook build --gitbook=2.0.1
```

**列出本地所有的gitbook版本**

```text
gitbook ls
```

**列出远程可用的gitbook版本**

```text
gitbook ls-remote
```

**安装对应的gitbook版本**

```text
gitbook fetch 标签/版本号
```

**更新到gitbook的最新版本**

```text
gitbook update
```

**卸载对应的gitbook版本**

```text
gitbook uninstall 2.0.1
```

**指定log的级别**

```text
gitbook build --log=debug
```

**输出错误信息**

```text
gitbook builid --debug
```

# **Gitbook 目录结构**

## GitBook 项目结构

GitBook使用简单的目录结构。在 [SUMMARY](https://link.zhihu.com/?target=https%3A//toolchain.gitbook.com/pages.html) （即 `SUMMARY.md` 文件）中列出的所有 Markdown / Asciidoc 文件将被转换为 HTML。多语言书籍结构略有不同。

一个基本的 GitBook 电子书结构通常如下：

```text
.
├── book.json
├── README.md
├── SUMMARY.md
├── chapter-1/
|   ├── README.md
|   └── something.md
└── chapter-2/
    ├── README.md
    └── something.md
```

GitBook 特殊文件的功能：

![img](https://pic3.zhimg.com/80/v2-c883f6a3ca6133ec91553ff73ff63a16_720w.jpg)

## 静态文件和图片

静态文件是在 `SUMMARY.md` 中未列出的文件。除非被忽略，否则所有静态文件都将复制到输出路径。

## 忽略文件和文件夹

GitBook将读取 `.gitignore`，`.bookignore` 和 `.ignore` 文件，以获取要过滤的文件和文件夹。这些文件中的格式遵循 `.gitignore` 的规则：

```text
# This is a comment

# Ignore the file test.md
test.md

# Ignore everything in the directory "bin"
bin/*
```

## 项目与子目录集成

对于软件项目，您可以使用子目录（如 `docs/` ）来存储项目文档的图书。您可以配置根选项来指示 GitBook 可以找到该图书文件的文件夹：

```text
.
├── book.json
└── docs/
    ├── README.md
    └── SUMMARY.md
```

在 `book.json` 中配置以下内容：

```text
{
    "root": "./docs"
}
```

## Summary

GitBook 使用 `SUMMARY.md` 文件来定义本书的章节和子章节的结构。 `SUMMARY.md` 文件用于生成本书的目录。

`SUMMARY.md` 的格式是一个链接列表。链接的标题将作为章节的标题，链接的目标是该章节文件的路径。

向父章节添加嵌套列表将创建子章节。

**简单示例：**

```text
# Summary

* [Part I](part1/README.md)
    * [Writing is nice](part1/writing.md)
    * [GitBook is nice](part1/gitbook.md)
* [Part II](part2/README.md)
    * [We love feedback](part2/feedback_please.md)
    * [Better tools for authors](part2/better_tools.md)
```

每章都有一个专用页面（`part#/README.md`），并分为子章节。

**锚点**

目录中的章节可以使用锚点指向文件的特定部分。

```text
# Summary

### Part I

* [Part I](part1/README.md)
    * [Writing is nice](part1/README.md#writing)
    * [GitBook is nice](part1/README.md#gitbook)
* [Part II](part2/README.md)
    * [We love feedback](part2/README.md#feedback)
    * [Better tools for authors](part2/README.md#tools)
```

**部分**

目录可以分为以标题或水平线 `----` 分隔的部分：

```text
# Summary

### Part I

* [Writing is nice](part1/writing.md)
* [GitBook is nice](part1/gitbook.md)

### Part II

* [We love feedback](part2/feedback_please.md)
* [Better tools for authors](part2/better_tools.md)

----

* [Last part without title](part3/title.md)
```

Parts 只是章节组，没有专用页面，但根据主题，它将在导航中显示。

## 页面

**Markdown 语法**

默认情况下，GitBook 的大多数文件都使用 Markdown 语法。 GitBook 推荐使用这种语法。所使用的语法类似于 [GitHub Flavored Markdown syntax](https://link.zhihu.com/?target=https%3A//guides.github.com/features/mastering-markdown/) 。

此外，你还可以选择 [AsciiDoc 语法](https://link.zhihu.com/?target=https%3A//toolchain.gitbook.com/syntax/asciidoc.html)。

**页面内容示例：**

```text
# Title of the chapter

This is a great introduction.

## Section 1

Markdown will dictates _most_ of your **book's structure**

## Section 2

...
```

**页面前言**

页面可以包含一个可选的前言。它可以用于定义页面的描述。前面的事情必须是文件中的第一件事，必须采取在三虚线之间设置的有效YAML的形式。这是一个基本的例子：

```text
---
description: This is a short description of my page
---

# The content of my page
...
```

## Glossary

允许您指定要显示为注释的术语及其各自的定义。根据这些术语，GitBook 将自动构建索引并突出显示这些术语。

`GLOSSARY.md` 的格式是 `h2` 标题的列表，以及描述段落：

```text
## Term
Definition for this term

## Another term
With it's definition, this can contain bold text
and all other kinds of inline markup ...
```

# **Gitbook 配置**

> GitBook 允许您使用灵活的配置自定义您的电子书。
> 这些选项在 `book.json` 文件中指定。对于不熟悉 JSON 语法的作者，您可以使用 [JSONlint](https://link.zhihu.com/?target=http%3A//jsonlint.com/) 等工具验证语法。

## 常规设置

![img](https://pic4.zhimg.com/80/v2-07155bb666bb8ac75c29168b3a41cd07_720w.jpg)

author

作者姓名，在[http://GitBook.com](https://link.zhihu.com/?target=http%3A//GitBook.com)上，这个字段是预先填写的。

例：

```text
"author" : "victor zhang"
```

description

电子书的描述，默认值是从 README 中提取出来的。在[http://GitBook.com](https://link.zhihu.com/?target=http%3A//GitBook.com)上，这个字段是预先填写的。

例：

```text
"description" : "Gitbook 教程"
```

direction

文本的方向。可以是 rtl 或 ltr，默认值取决于语言的值。

例：

```text
"direction" : "ltr"
```

gitbook

应该使用的GitBook版本。使用SemVer规范，接受类似于 >=3.0.0 的条件。

例：

```text
"gitbook" : "3.0.0",
"gitbook" : ">=3.0.0"
```

language

Gitbook使用的语言, 版本2.6.4中可选的语言如下：

```text
en, ar, bn, cs, de, en, es, fa, fi, fr, he, it, ja, ko, no, pl, pt, ro, ru, sv, uk, vi, zh-hans, zh-tw
```

例：

```text
"language" : "zh-hans",
```

links

在左侧导航栏添加链接信息

例：

```text
"links" : {
    "sidebar" : {
        "Home" : "https://github.com/dunwu/gitbook-notes"
    }
}
```

root

包含所有图书文件的根文件夹的路径， book.json 文件除外。

例：

```text
"root" : "./docs",
```

structure

指定 Readme、Summary、Glossary 和 Languages 对应的文件名。

styles

自定义页面样式， 默认情况下各generator对应的css文件

例：

```text
"styles": {
    "website": "styles/website.css",
    "ebook": "styles/ebook.css",
    "pdf": "styles/pdf.css",
    "mobi": "styles/mobi.css",
    "epub": "styles/epub.css"
}
```

例如要使 `h1`、`h2` 标签有下边框， 可以在 `website.css` 中设置

```text
h1 , h2{
    border-bottom: 1px solid #EFEAEA;
}
```

title

电子书的书名，默认值是从 README 中提取出来的。在 [http://GitBook.com](https://link.zhihu.com/?target=http%3A//GitBook.com) 上，这个字段是预先填写的。

例：

```text
"title" : "gitbook-notes",
```

# plugins

插件及其配置在 `book.json` 中指定。有关详细信息。

自 3.0.0 版本开始，GitBook 可以使用主题。有关详细信息，请参阅 [the theming section](https://link.zhihu.com/?target=https%3A//toolchain.gitbook.com/themes/) 。

![img](https://pic1.zhimg.com/80/v2-13903cf4011fa33688d4d9822454e6e4_720w.jpg)

## 添加插件

```text
"plugins": [
    "splitter"
]
```

添加新插件之后需要运行 `gitbook install` 来安装新的插件

## 去除自带插件

Gitbook 默认带有 5 个插件：

- highlight
- search
- sharing
- font-settings
- livereload

```text
"plugins": [
    "-search"
]
```

## structure

除了 `root` 属性之外，您可以指定 Readme，Summary，Glossary 和 Languages 的名称（而不是使用默认名称，如README.md）。这些文件必须在项目的根目录下（或 `root` 的根目录，如果你在 `book.json` 中配置了 `root` 属性）。不接受的路径，如：`dir / MY_README.md`。

![img](https://pic2.zhimg.com/80/v2-36344c71c4dafd92e86a88a4b9ad0951_720w.jpg)

## pdf

可以使用 `book.json` 中的一组选项来定制PDF输出：

![img](https://pic1.zhimg.com/80/v2-7021e3f05aeaa99e0ebebee3e10473f0_720w.jpg)

# **生成电子书**

GitBook 可以生成一个网站，但也可以输出内容作为电子书（ePub，Mobi，PDF）。

```text
# Generate a PDF file
$ gitbook pdf ./ ./mybook.pdf

# Generate an ePub file
$ gitbook epub ./ ./mybook.epub

# Generate a Mobi file
$ gitbook mobi ./ ./mybook.mobi
```

## 安装 ebook-convert

`ebook-convert` 可以用来生成电子书（epub，mobi，pdf）。

## GNU/Linux

安装 [Calibre application](https://link.zhihu.com/?target=https%3A//calibre-ebook.com/download).

```text
$ sudo aptitude install calibre
```

在一些 GNU / Linux 发行版中，节点被安装为 nodejs，您需要手动创建一个符号链接：

```text
$sudo ln -s /usr/bin/nodejs /usr/bin/node
```

## 封面

封面用于所有电子书格式。您可以自己提供一个，也可以使用 [autocover plugin](https://link.zhihu.com/?target=https%3A//plugins.gitbook.com/plugin/autocover) 生成一个。

要提供封面，请将 `cover.jpg` 文件放在书本的根目录下。添加一个 `cover_small.jpg` 将指定一个较小版本的封面。封面应为 `JPEG` 文件。

好的封面应该遵守以下准则：

- `cover.jpg` 的尺寸为 1800x2360 像素，`cover_small.jpg` 为 200x262
- 没有边界
- 清晰可见的书名
- 任何重要的文字应该在小版本中可见