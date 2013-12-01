快速入门
########

安装
====

Pelican目前基于Python2.7.x 环境运行最佳；暂时支持Python3.3，但是可能会出现不可预知的问题，尤其是关于可选的第三方组件。

可以使用多种方法安装Pelican，最简洁的方法则是通过 `pip <http://www.pip-installer.org/>`_::

    $ pip install pelican

如果您并未安装 ``pip`` 命令，也可以使用 ``easy_install``::

    $ easy_install pelican

（记住：为了便于安装Pelican，操作系统将会经常提醒您加上 ``sudo`` 前缀。）

虽然上面的方法十分简单，但是仍然推荐使用虚拟环境安装Pelican，如果您已经安装 virtualenv_ ，则可以打开终端并且为Pelican创建一个新的虚拟环境::

    $ virtualenv ~/virtualenvs/pelican
    $ cd ~/virtualenvs/pelican
    $ . bin/activate

一旦虚拟环境创建并激活，您就可以通过上面提到的pip命令来安装Pelican。如果您拥有项目源码，还可以通过distutils方法来安装Pelican::

    $ cd path-to-Pelican-source //Pelican源代码路径
    $ python setup.py install

如果您已经安装Git并且希望安装最新版的Pelican，使用以下命令::

    $ pip install -e git+https://github.com/getpelican/pelican.git#egg=pelican

如果您计划使用Markdown语法格式，您需要安装如下的Markdown库::

    $ pip install Markdown

如果您想使用 AsciiDoc_ 语法格式，则需要从 `项目源 <http://www.methods.co.nz/asciidoc/INSTALL.html>`_ ，或者使用操作系统的包管理安装 AsciiDoc_ 。

基本用法
--------

只要安装Pelican，就可以通过 ``pelican`` 命令将Markdown或者reST等内容翻译成HTML格式，可以随意指定内容路径以及settings.py文件路径::

$ pelican /path/to/your/content/ [-s path/to/your/settings.py]

通过以上命令可以在 ``output/`` 目录下，使用默认主题制作一个简单的网站。默认的主题包括较简单的HTML格式，非常普通，不具备任何风格，但是您可以以此为基础制作专属主题。

可以通过设置Pelican对您所作的修改进行监视，而无须每次想要查看变化时，都要手动地重新运行。运行 ``pelican -r`` 或者 ``pelican --autoreload`` 命令可以设置。

Pelican命令行包含其它选项，可以通过--help命令查看::

    $ pelican --help

继续阅读更多细节，请查阅Pelican在社区发布的参考文档 `Tutorials <https://github.com/getpelican/pelican/wiki/Tutorials>`_ 。

查看生成的文件
--------------

Pelican生成静态文件，所以您可以使用浏览器直接打开HTML文件::

    firefox output/index.html

由于以上方法在载入CSS或者其他链接资源时可能会产生错误，因此使用Python运行一个简单的Web服务器，往往会提供更加可靠的预览体验::

    cd output && python -m SimpleHTTPServer

一旦 ``SimpleHTTPServer`` 启动, 则可以通过 http://localhost:8000/ 浏览您的网站

更新
----

如果您通过 ``pip`` 或者 ``easy_install`` 方法安装稳定版的Pelican，并且希望升级到最新稳定版，您可以在相关命令之后添加 ``--upgrade`` 进行升级，例如pip安装方式，升级命令如下::

    $ pip install --upgrade pelican

如果您是通过distutils或者源代码方法安装Pelican，简单地执行相同的步骤，安装最新版本即可。

依赖包
------

Pelican安装之后，依赖于以下的Python包都将会自动安装：

* `feedgenerator <http://pypi.python.org/pypi/feedgenerator>`_, 生成Atom
* `jinja2 <http://pypi.python.org/pypi/Jinja2>`_, 模板支持
* `pygments <http://pypi.python.org/pypi/Pygments>`_, 语法高亮
* `docutils <http://pypi.python.org/pypi/docutils>`_, 支持reStructuredText输入格式
* `pytz <http://pypi.python.org/pypi/pytz>`_, 定义时区
* `blinker <http://pypi.python.org/pypi/blinker>`_, 消息分发系统
* `unidecode <http://pypi.python.org/pypi/Unidecode>`_, Unicode文件转码
* `six <http://pypi.python.org/pypi/six>`_,  兼容Python2、3
* `MarkupSafe <http://pypi.python.org/pypi/MarkupSafe>`_, 安全标记字符串

如果需要以下安装包，可以通过 ``pip`` 命令手动安装：

* `markdown <http://pypi.python.org/pypi/Markdown>`_, 支持Markdown格式输入
* `typogrify <http://pypi.python.org/pypi/typogrify>`_, 增强排版

快速部署您的网站
================

Pelican安装之后，您可以通过 ``pelican-quickstart`` 命令创建一个项目框架，需要按照提示要求输入一些内容::

    $ pelican-quickstart

安装完毕，您的项目将包含以下目录树（除了“pages”目录，如果您计划创建按照时间序列排序的内容，您可以选择性地自己添加）::

    yourproject/
    ├── content
    │   └── (pages)
    ├── output
    ├── develop_server.sh
    ├── fabfile.py
    ├── Makefile
    ├── pelicanconf.py       # 主要设置文件
    └── publishconf.py       # 文章发布设置

下一步就是要开始向您创建的 *content* 目录添加内容。（请参考 **Writing content using Pelican** 小节内容，查看如何格式化写作内容。）

如果您已经写好文章准备生成html，你可以使用 ``pelican`` 命令在output目录生成对应的网页。

自动化工具
==========

可以使用 ``pelican`` 命令结合自动化工具简化生成和发布的流程。在 ``pelican-quickstart`` 命令执行时，涉及是否要自动生成并发布页面等相关问题。如果你回答“是”，  将会在您创建的项目根目录下生成 ``fabfile.py`` 和 ``Makefile`` 文件。这些文件会记录安装过程中所做的相关设置，因此从一开始，就应该根据特定需求及其使用模式来设置相关信息。如果你发现某些自动化工具使用受限，可以随时删除这些文件，并不会影响使用标准的 ``pelican`` 命令。

以下是“围绕” ``pelican`` 命令的自动化工具，可以简化生成，预览和上传网站的过程。

Fabric
------

Fabric_ 的优点在于：使用Python编写，应用环境更为广泛。缺点是必须单独安装，使用如下命令安装Fabric，如果环境需要，需加上 ``sudo`` 前缀::

    $ pip install Fabric

稍后打开在项目根目录下生成的 ``fabfile.py`` 文件，你将会发现一系列命令，其中任何一个都可以重命名、删除或者根据喜好自定义。使用现成配置，通过以下途径生成网页::

    $ fab build

如果每次检测到变化时，你希望Pelican会自动重新生成页面（本地测试都是基于手动生成），可以使用如下命令::

    $ fab regenerate

为生成的页面启动服务，通过浏览器访问 http://localhost:8000/ 进行预览::

    $ fab serve

如果在pelican-quickstart过程中，当提及是否通过SSH上传你的页面，你选择回答“yes”，那么你可以通过SSH同步使用以下命令来发布页面::

    $ fab publish

这些只是默认的一些可用命令，所以自行研究 ``fabfile.py`` 文件，查看其它可用命令。更重要的是，您可以定制 ``fabfile.py`` 文件适应特定需求和喜好。

生成
----

在pelican安装过程中，如果你对某些问题回答“yes”， ``Makefile`` 文件则会自动创建，该方法的优点在于 ``make`` 命令内置在很多POSIX系统（unix、linux）中，无需安装其它组件即可使用。缺点是非POSIX系统（例如windows）并未包含 ``make`` 命令，在此类系统上安装make命令将会变得十分困难。

如果你想使用 ``make`` 命令生成网页，请运行::

    $ make html

如果每次检测到变化时，你希望Pelican会自动重新生成页面（本地测试都是基于手动生成），可以使用如下命令::

    $ make regenerate

为生成的页面启动服务，通过浏览器访问 http://localhost:8000/ 进行预览::

    $ make serve

正常情况下，需要单独运行 ``make`` 命令和 ``make serve`` 命令生成页面，但是现在你可以通过 ``make regenerate`` 命令同时运行::

    $ make devserver

以上命令可以同时生成网页并开启服务，可以直接通过 http://localhost:8000 访问，如果您完成测试更改，通过以下命令停止服务::

    $ ./develop_server.sh stop

准备发布页面时，可以通过在pelican初始安装时选择的上传方法，例如，我们将使用ssh来同步::

    $ make rsync_upload

如上所示，您的网页应该可以访问了。

使用Pelican写作
===============

文章和页面
----------

Pelican认为“文章”是按时间顺序自动排列的，例如博客上的文章，也是以日期来归类。

“页面”背后的出发点是：
“页面”背后的想法是，作者们通常选择固定的或者是不经常改变的内容（比如“关于我们”或者“联系我们”等页面）

.. _internal_metadata:

文件元数据
----------

Pelican试图更加智能的获取文件系统所需信息（例如，关于文章的分类），但是有些信息必须在文件中以元数据的形式体现。

如果你以reStructuredText格式写作，可以通过以下的语法格式，在文本文件中提供类似的元数据（文件以 ``.rst`` 结尾）::

    My super title
    ##############

    :date: 2010-10-03 10:20
    :modified: 2010-10-04 18:40
    :tags: thats, awesome
    :category: yeah
    :slug: my-super-post
    :author: Alexis Metaireau
    :summary: Short version for index and feeds

Pelican针对reStructuredText语法，实现支持HTML缩进标签的扩展，如果使用，格式如下::

    This will be turned into :abbr:`HTML (HyperText Markup Language)`.

你也可以使用Markdown语法（文件后缀以 ``.md`` 、 ``.mkd`` 或者 ``.mdown`` 结尾）。Markdown格式的生成需要首先安装 ``Markdown`` 包，可以通过 ``pip install Markdown`` 命令安装，Markdown元数据语法格式应该如下所示::

    Title: My super title
    Date: 2010-12-03 10:20
    Modified: 2010-12-05 19:30
    Category: Python
    Tags: pelican, publishing
    Slug: my-super-post
    Author: Alexis Metaireau
    Summary: Short version for index and feeds

    This is the content of my super blog post.

如果以 AsciiDoc_ 写作，应当以 ``.asc`` 为后缀，可以参考 AsciiDoc_ 网站。

Pelican可以生成以 ``.html`` 或者 ``.htm`` 结尾的HTML文件，直接解析HTML文件，从meta标签读取元数据，从title标签读取标题，从body标签读取内容::

    <html>
        <head>
            <title>My super title</title>
            <meta name="tags" content="thats, awesome" />
            <meta name="date" content="2012-07-09 22:28" />
            <meta name="modified" content="2012-07-10 20:14" />
            <meta name="category" content="yeah" />
            <meta name="author" content="Alexis Métaireau" />
            <meta name="summary" content="Short version for index and feeds" />
        </head>
        <body>
            This is the content of my super blog post.
        </body>
    </html>

对于HTML来说，具有一种异常简单的标准元数据：通过符合Pelican标准的tags元数据，tags可以被指定，或者通过符合HTML标准的keywords元数据，两种方式可以交替使用。

注意，除了标题以外，文章的元数据是没有强制性的：如果没有指定日期， ``DEFAULT_DATE`` 被设置为 ``fs`` ，Pelican将依赖文件的“mtime”时间戳，类别可以通过文件所在的目录来决定。例如，某个文件位于 ``python/foobar/myfoobar.rst`` ，则具有 ``foobar`` 的类别。如果你想通过子文件夹名称作为类别名来组织你的文件，这并非是个好主意。你可以将 ``USE_FOLDER_AS_CATEGORY`` 属性设置为 ``False`` ，在页面元数据中解析日期时，Pelican支持 `W3C ISO 8601`_ 。

如果你不明确指定给定文章摘要的元数据，SUMMARY_MAX_LENGTH设置将会指定文章开头的某某字数作为摘要。

您也可以设置 ``FILENAME_METADATA`` 属性，通过正则表达式提取任何文件名的元数据。所有匹配的命名组将会被设置在元数据对象中。 ``FILENAME_METADATA`` 默认设置将只从文件名提取日期。例如，如果你想提取date和slug，你可以这样设置： ``'(?P<date>\d{4}-\d{2}-\d{2})_(?P<slug>.*)'`` 

Please note that the metadata available inside your files takes precedence over
the metadata extracted from the filename.
请注意，在文件中可用的元数据优先级高于从文件名中提取的元数据。

页面
-----

如果在content目录下创建一个 ``pages`` 文件夹，该文件夹内所有文件将会以静态页面生成，例如 **About** 或者 **Contact** 页面。（例如下面的文件系统布局）

使用 ``DISPLAY_PAGES_ON_MENU`` 设置控制是否在导航栏中显示这些页面（默认设置为 ``True`` ）

如果你不希望在菜单中链接或者列出任何页面，需要添加如下状态：设置该元数据的隐藏属性。这对于设置错误页面（404）很有帮助。

.. _ref-linking-to-internal-content:

链接到内部内容
---------------------------

从Pelican3.1版本开始，可以指定站点内部链接文件的源content目录层次，而不是文件生成的结构层次。建立当前文字与 其他相邻的文字和图片之间的链接将变得相当容易。（而不是具有确定这些资源在站点生成后如何放置）

要链接到content目录内部（在 ``content`` 目录下的所有文件），使用以下语法： ``{filename}path/to/file``::


    website/
    ├── content
    │   ├── article1.rst
    │   ├── cat/
    │   │   └── article2.md
    │   └── pages
    │       └── about.md
    └── pelican.conf.py

在该案例中， ``article1.rst`` 格式应如下所示::

    The first article
    #################

    :date: 2012-12-01 10:02

    See below intra-site link examples in reStructuredText format.

    `a link relative to content root <{filename}/cat/article2.rst>`_
    `a link relative to current file <{filename}cat/article2.rst>`_

and ``article2.md``::

    Title: The second article
    Date: 2012-12-01 10:02

    See below intra-site link examples in Markdown format.

    [a link relative to content root]({filename}/article1.md)
    [a link relative to current file]({filename}../article1.md)

与嵌入non-article或者non-page content目录略有不同，需要在 ``pelicanconf.py`` 文件中指定， ``images`` 目录默认配置如下，其他的需要手动添加::

    content
    ├── images
    │   └── han.jpg
    └── misc
        └── image-test.md

And ``image-test.md`` would include::

    ![Alt Text]({filename}/images/han.jpg)

这种方式可以链接任何内容，由于Pelican默认设置 ``STATIC_PATHS`` 包括 ``images`` ，所以在网页生成的同时， ``images`` 目录将会被复制到 ``output/`` 目录下。如果期望更换其他目录，比如 ``pdfs`` 目录，在页面生成的同时从content目录拷贝文件到output目录，需要将以下内容添加到设置文件中::

    STATIC_PATHS = ['images', 'pdfs']

上面一行内容添加之后，随后的页面生成应该复制 ``content/pdfs/`` 目录到 ``output/pdfs/`` 目录下。

你还可以链接到分类或者标签，使用 ``{tag}标签名`` 和 ``{category}foobar`` 语法。

为了向后兼容，Pelican除了大括号以外（ ``{}`` ）还支持双竖线（ ``||`` ）。例如： ``|filename|an_article.rst``,  ``|tag|tagname`` ,  ``|category|foobar`` .语法格式从 ``||`` 过渡到到 ``{}`` ，主要是为了避免与Markdown扩展或者reST指令冲突。

导入现有的博客
--------------------------

支持从Dotclear, WordPress, 以及RSS来导入博客内容，只需使用一段简单的脚本即可。查阅后面章节内容，了解从其他博客软件导入过程，查看 :ref:`import` 。

翻译
----

支持文章翻译。要做到这一点，需要增加文章/页面的 ``lang``  meta属性，设置 ``DEFAULT_LANG`` 属性（默认为[en]，英语）。使用这些设置，只有默认语言的文章会被发布，每篇文章都会伴随着可用的翻译列表。

Pelican使用文章的URL中的 "slug" 属性检测文章之间是否相互翻译。slug可以在文件的元数据中手动设置；如果没有明确设定，Pelican将会根据文章的标题自动生成slug。

参考如下两篇文章，一篇英文文章，另一篇为法语文章。

英文文章::

    Foobar is not dead
    ##################

    :slug: foobar-is-not-dead
    :lang: en

    That's true, foobar is still alive!

法语文章::

    Foobar n'est pas mort !
    #######################

    :slug: foobar-is-not-dead
    :lang: fr

    Oui oui, foobar est toujours vivant !

这两篇文章之间，尽管发布的内容不同，仍有唯一的相同点——slug（在此处用作识别符）。如果你并未明确定义slug，必须要确保不同语言版本文章的标题是相同的，因为slug将会根据文章标题自动生成。

如果你不希望某篇文章的原始版本被 ``DEFAULT_LANG`` 设置检测到，需要使用 ``translation`` 元数据指定哪些内容是译文::

    Foobar is not dead
    ##################

    :slug: foobar-is-not-dead
    :lang: en
    :translation: true

    That's true, foobar is still alive!


.. _internal_pygments_options:

语法高亮
--------

Pelican可以为代码块提供不同的语法高亮，需要在文件内容中使用如下约定：

以reStructuredText格式为例，使用代码块指令::

    .. code-block:: identifier

       <indented code block goes here>

以Markdown为例，包括语言识别，类似上面的代码块，缩进的标识符和代码::

    A block of text.

        :::identifier
        <code goes here>

指定标识符（例如 ``python`` ， ``ruby`` ）应该是出现在列表上可用的语法解析器 `list of available lexers <http://pygments.org/docs/lexers/>`_.

当使用reStructuredText语法时，以下选项可用:

=============   ============  =========================================
选项             有效值           说明
=============   ============  =========================================
anchorlinenos   N/A           If present wrap line numbers in <a> tags.
classprefix     string        String to prepend to token class names
hl_lines        numbers       List of lines to be highlighted.
lineanchors     string        Wrap each line in an anchor using this
                              string and -linenumber.
linenos         string        If present or set to "table" output line 
                              numbers in a table, if set to
                              "inline" output them inline. "none" means
                              do not output the line numbers for this 
                              table.
linenospecial   number        If set every nth line will be given the 
                              'special' css class.
linenostart     number        Line number for the first line.
linenostep      number        Print every nth line number.
lineseparator   string        String to print between lines of code,
                              '\n' by default.
linespans       string        Wrap each line in a span using this and
                              -linenumber.
nobackground    N/A           If set do not output background color for
                              the wrapping element
nowrap          N/A           If set do not wrap the tokens at all.
tagsfile        string        ctags file to use for name definitions.
tagurlformat    string        format for the ctag links.
=============   ============  =========================================

注意：Pygments模块可能没有这些可用选项，这主要取决于不同的版本。请参阅 `Pygments 文档 <http://pygments.org/docs/formatters/>`_ 中 *HtmlFormatter* 部分，获取更多细节。

例如，下面的代码块能够显示行号，从153开始，以Pygments格式的CSS类（ *pgcss* ）使得名称更为独特，避免跟默认的CSS格式冲突::

    .. code-block:: identifier
        :classprefix: pgcss
        :linenos: table
        :linenostart: 153

       <indented code block goes here>

也可以在Pelican设置文件中指定 ``PYGMENTS_RST_OPTIONS`` 变量和选项，这将自动应用到每一个代码块。

例如，如果你想为代码块显示行号，并且显示CSS前缀，变量设置如下::

    PYGMENTS_RST_OPTIONS = {'classprefix': 'pgcss', 'linenos': 'table'}

如果指定，个人代码块设置将会覆盖默认设置文件中的值。

发布草稿
--------

如果你想将文章作为草稿发布（例如：在正式发布之前审核一下），可以增加一条状态：元数据属性为草稿。那么该文章将会以 ``drafts`` 文件夹的形式输出，在首页或者其他类别以及标签页中并未显示。

.. _virtualenv: http://www.virtualenv.org/
.. _W3C ISO 8601: http://www.w3.org/TR/NOTE-datetime
.. _Fabric: http://fabfile.org/
.. _AsciiDoc: http://www.methods.co.nz/asciidoc/
