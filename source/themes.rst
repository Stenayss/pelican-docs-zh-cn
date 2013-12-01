.. _theming-pelican:

主题制作
########

Pelican使用伟大的 `Jinja2 <http://jinja.pocoo.org/>`_ 模板引擎嵌入到输出的HTML页面中，Jinja2语法十分简单，如果你想制作自己的主题，可以从这类 `"simple" 的主题中随时汲取灵感 <https://github.com/getpelican/pelican/tree/master/pelican/themes/simple/templates>`_ 。

结构
====

制作个人主题，必须遵守以下目录结构::

    ├── static
    │   ├── css
    │   └── images
    └── templates
        ├── archives.html         // 显示归类
        ├── period_archives.html  // 按照日期归类
        ├── article.html          // 文章处理
        ├── author.html           // 作者处理
        ├── authors.html          // 列出所有文章的作者
        ├── categories.html       // 分类汇总
        ├── category.html         // 分类处理
        ├── index.html            // 首页显示所有文章
        ├── page.html             // 页面处理
        ├── tag.html              // 标签处理
        └── tags.html             // 标签汇总，生成云标签

* `static` 目录包含所有的静态资源，将被复制到output目录 `theme` 文件夹，以上文件包括CSS以及image目录，这只是案例，可以随心所欲定制。

* `templates` 目录包含所有模板文件，上面列出的模板文件是强制性的，你也可以添加自己的模板。

模板以及变量
============

该思路语法简单，嵌入到HTML页面中，该文档描述主题中应该存在的模板，以及在生成页面的同时，哪些变量会被传递给模板。

所有的模板文件都将接收到大写的自定义变量，可以直接访问。

全局变量
--------

以下设置针对所有模板具有通用性。

=============   ===================================================
变量              说明
=============   ===================================================
output_file     目前正在生成的文件名称，例如，当Pelican进行网页渲染时，output_file则为 "index.html"
articles        按照日期降序排列的文章列表, 所有的元素都是 `Article` 对象, 因此可以访问其属性 (e.g. title, summary, author etc.). 有时也会带来不便 (例如在tags页面). 你会在 `all_articles` 变量中发现它的信息。
dates           按照日期升序排列的文章列表。
tags            （标签和文章）元组列表，包含所有tags
categories      （分类和文章）元组列表，包含所有分类和相关文章（的值）
pages           页面列表
=============   ===================================================

排序
----

采取比较的方法进行URl封装（当前分类、标签和作者），使得它们易于按照名称排序::

    {% for tag, articles in tags|sort %}

如果你想基于不同的标准排序, Jinja的排序命令 `Jinja's sort
command`__ 有多项选择。

__ http://jinja.pocoo.org/docs/templates/#sort


日期格式
--------

根据设置以及语言环境进行日期格式化 (``DATE_FORMATS``/``DEFAULT_DATE_FORMAT``) ，并且提供了 ``locale_date`` 属性。此外， ``date`` 属性将是 `datetime`_ 对象，如果你需要自定义日期格式，使用Jinja filter  ``strftime`` ，用法和python的 `strftime`_ 格式类似，filter会根据语言环境设置格式化日期::

    {{ article.date|strftime('%d %B %Y') }}

.. _datetime: http://docs.python.org/2/library/datetime.html#datetime-objects
.. _strftime: http://docs.python.org/2/library/datetime.html#strftime-strptime-behavior

index.html
----------

博客的首页，生成位置：output/index.html

如果分页可用，子页面将会保存为 output/index`n`.html.

===================     ===================================================
变量                     说明
===================     ===================================================
articles_paginator      文章列表分页对象
articles_page           当前文章页面
dates_paginator         文章列表分页对象, 按照日期升序排列
dates_page              文章列表分页对象, 按照日期升序排列
page_name               'index' -- 有效的分页链接
===================     ===================================================

author.html
-------------

该模板针对作者进行处理，输出位置： output/author/`author_name`.html

如果分页可用，子页面将会根据 AUTHOR_SAVE_AS 设置进行保存 (`Default:` output/author/`author_name'n'`.html)

===================     ===================================================
变量                      说明
===================     ===================================================
author                  作者
articles                此作者的所有文章
dates                   此作者的所有文章, 按照日期升序排列
articles_paginator      文章列表分页对象
articles_page           当前文章页面
dates_paginator         文章列表分页对象, 按照日期升序排列
dates_page              当前文章页面, 按照日期升序排列
page_name               AUTHOR_URL  `{slug}` 之后的内容将被删除 -- 有利于分页链接
===================     ===================================================

category.html
-------------

该模板将会针对每个存在的分类进行处理，输出位置：output/category/`category_name`.html.

如果分页可用，子页面将会根据 CATEGORY_SAVE_AS 设置进行保存 (`Default:` output/category/`category_name'n'`.html)

===================     ===================================================
变量                      说明
===================     ===================================================
category                处理分类名称
articles                文章分类
dates                   文章分类, 按照日期升序排列
articles_paginator      文章列表分页对象
articles_page           当前文章页面
dates_paginator         文章列表分页对象,按照日期升序排列
dates_page              当前文章页面, 按照日期升序排列
page_name               CATEGORY_URL  `{slug}` 之后的内容将被删除 -- 有利于分页链接
===================     ===================================================

article.html
-------------

该模板将会处理每一篇文章，保存.html文件位置：output/`article_name`.html，以下是得到的具体变量。

=============   ===================================================
变量             说明
=============   ===================================================
article         显示文章对象
category        当前文章分类名
=============   ===================================================

在文章源文件件中，任何header头部元数据都将作为 ``article`` 对象的可用字段。处了全小写字符以外，该字段名与元数据字段名相同。

例如，在文章元数据中添加 `FacebookImage` 字段，如下所示::

.. code-block:: markdown

    Title: I love Python more than music
    Date: 2013-11-06 10:06
    Tags: personal, python
    Category: Tech
    Slug: python-je-l-aime-a-mourir
    Author: Francis Cabrel
    FacebookImage: http://franciscabrel.com/images/pythonlove.png

新的元数据在 `article.html` 模板中为 `article.facebookimage`  。可以在Facebook open graph标签中提供图像，更改每一篇文章::

.. code-block:: html+jinja

    <meta property="og:image" content="{{ article.facebookimage }}"/>


page.html
---------

该模板将会处理每一个页面，保存相关的.html文件位置： output/page_name.html。

=============   ===================================================
变量              说明
=============   ===================================================
page            显示页面对象. 可以访问它的title, slug, 以及 content.
=============   ===================================================

tag.html
--------

模板会处理每一个标签，保存相关的.html文件路径为： output/tag/`tag_name`.html。

如果分页可用，子页面将会根据 TAG_SAVE_AS 设置进行保存 (`Default:` output/tag/`tag_name'n'`.html)

===================     ===================================================
变量                       说明
===================     ===================================================
tag                     处理标签名
articles                与此标签相关的文章
dates                   与此标签相关的文章, 按照日期升序排列
articles_paginator      文章列表分页对象
articles_page           当前文章页面
dates_paginator         文章列表分页对象,按照日期升序排列
dates_page              当前文章页面, 按照日期升序排列
page_name               TAG_URL  `{slug}` 之后的内容将被删除 -- 有利于分页链接
===================     ===================================================

Feeds
=====

feed变量在3.0版本中发生变化，现在每个变量都明确地列出ATOM或者RSS的名称。默认依然为ATOM，较老的主题需要及时更新，以下是一份完整的feed变量::

    FEED_ATOM
    FEED_RSS
    FEED_ALL_ATOM
    FEED_ALL_RSS
    CATEGORY_FEED_ATOM
    CATEGORY_FEED_RSS
    TAG_FEED_ATOM
    TAG_FEED_RSS
    TRANSLATION_FEED_ATOM
    TRANSLATION_FEED_RSS



继承
====

自从3.0版本开始，Pelican支持继承 ``simple`` 主题，因此你可以在自定义主题中复用 ``simple`` 主题模板。

如果 ``templates/`` 目录下某个关键性文件丢失，将会使用 ``simple`` 主题中的模板进行匹配替换。因此如果 ``simple`` 主题中的HTML模板结构适合你，则无需重新编写新的模板。

你还可以使用 ``{% extends %}`` 指令，在自定义主题中根据 ``simple`` 主题模板进行扩展，如下所示::

.. code-block:: html+jinja

    {% extends "!simple/index.html" %}   <!-- extends the ``index.html`` template from the ``simple`` theme -->

    {% extends "index.html" %}   <!-- "regular" extending -->


实例
----

可以简单的使用两个文件创建主题。

base.html
"""""""""

第一个文件： ``templates/base.html`` 模板:

.. code-block:: html+jinja

    {% extends "!simple/base.html" %}

    {% block head %}
    {{ super() }}
       <link rel="stylesheet" type="text/css" href="{{ SITEURL }}/theme/css/style.css" />
    {% endblock %}

1. 第一行，基于 ``simple`` 主题，扩展 ``base.html`` 模板页，不必重写该文件。
2. 第三行，打开 ``simple`` 主题中已定义的 ``head`` 块。
3. 第四行，函数 ``super()`` 在 ``head`` 块中插入内容。
4. 第五行，为页面添加样式列表。
5. 最后一行，关闭 ``head`` 块。

可以基于该文件进行扩展，所有页面都可以使用该样式列表（stylesheet）。

style.css
"""""""""

第二个文件则是CSS样式表： ``static/css/style.css`` 

.. code-block:: css

    body {
        font-family : monospace ;
        font-size : 100% ;
        background-color : white ;
        color : #111 ;
        width : 80% ;
        min-width : 400px ;
        min-height : 200px ;
        padding : 1em ;
        margin : 5% 10% ;
        border : thin solid gray ;
        border-radius : 5px ;
        display : block ;
    }

    a:link    { color : blue ; text-decoration : none ;      }
    a:hover   { color : blue ; text-decoration : underline ; }
    a:visited { color : blue ;                               }

    h1 a { color : inherit !important }
    h2 a { color : inherit !important }
    h3 a { color : inherit !important }
    h4 a { color : inherit !important }
    h5 a { color : inherit !important }
    h6 a { color : inherit !important }

    pre {
        margin : 2em 1em 2em 4em ;
    }

    #menu li {
        display : inline ;
    }

    #post-list {
        margin-bottom : 1em ;
        margin-top : 1em ;
    }

下载
""""

样例下载： :download:`here <_static/theme-basic.zip>`.
