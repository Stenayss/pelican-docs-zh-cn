设置
####

可以通过配置文件配置Pelican，使用如下命令指定配置文件路径::

    $ pelican -s path/to/your/settingsfile.py path

设置文件以Python模块（单文件）形式存在，可以查看样例 `/samples/pelican.conf.py
<https://github.com/getpelican/pelican/raw/master/samples/pelican.conf.py>`_

所有的设置标识符必须全部大写，否则无法识别。可以使用数字（例如5,20等）、波尔类型（True, False, None等）、字典或者元组，这些值不加引号，其他值（例如字符串）必须包含在引号内。

除非另作说明，在配置文件中可以设置绝对路径或相对路径。

所有配置选项都会传递给模板文件，允许使用自定义设置添加整站内容。

列表设置如下：

基本设置
========

=====================================================================   =====================================================================
配置名称 (默认值)                                                       说明
=====================================================================   =====================================================================
`AUTHOR`                                                                默认作者（输入您的名字）
`DATE_FORMATS` (``{}``)                                                 如果您使用多种语言，可以在此设置日期格式，详情参加“日期格式与区域设置”
`USE_FOLDER_AS_CATEGORY` (``True``)                                     如果您不希望在文章元数据中指定类别，将该项设置为 ``True`` ，利用子目录安排文章结构，子目录将成为文章的分类，如果设置为 ``False`` ， ``DEFAULT_CATEGORY`` 将作为备用选项。
`DEFAULT_CATEGORY` (``'misc'``)                                         默认文章分类
`DEFAULT_DATE_FORMAT` (``'%a %d %B %Y'``)                               默认日期格式
`DISPLAY_PAGES_ON_MENU` (``True``)                                      是否在模板菜单上显示页面，可以在模板中选择是否使用该配置项
`DISPLAY_CATEGORIES_ON_MENU` (``True``)                                 是否在模板菜单上显示分类，可以在模板中选择是否使用该配置项
`DEFAULT_DATE` (``None``)                                               如果设为“fs”，当无法从元数据中获取日期信息时，Pelican将会使用文件系统时间戳信息（mtime），如果设为元组对象，默认的datetime对象将通过元组的构造方法生成
`DEFAULT_METADATA` (``()``)                                             文章和页面的默认元数据设置
`FILENAME_METADATA` (``'(?P<date>\d{4}-\d{2}-\d{2}).*'``)               使用正则表达式提取文件名的元数据，在元数据对象中设置用来匹配所有组。默认只能从文件名中提取日期，如果你想同时提取date和slug，设置如下：  ``'(?P<date>\d{4}-\d{2}-\d{2})_(?P<slug>.*)'``. See :ref:`path_metadata`. 
`PATH_METADATA` (``''``)                                                例如 ``FILENAME_METADATA``, 从一个页面的完整路径相对于内容源目录进行解析
                                                                        See :ref:`path_metadata`.
`EXTRA_PATH_METADATA` (``{}``)                                          通过相对路径添加元数据字典关键字
                                                                        See :ref:`path_metadata`.
`DELETE_OUTPUT_DIRECTORY` (``False``)                                   删除putput目录，优点在于避免生成不必要的文件，同时， **该设置具有一定风险，请谨慎处理。**
`OUTPUT_RETENTION` (``()``)                                             在output目录中应该保留元组文件名，典型的案例则是用于保存版本控制数据。例如 ``(".hg", ".git", ".bzr")``
`JINJA_EXTENSIONS` (``[]``)                                             使用Jinja扩展列表
`JINJA_FILTERS` (``{}``)                                                Jinja2 filters自定义列表，字典应该映射filter函数的filtername。例如: ``{'urlencode': urlencode_filter}``
                                                                        See `Jinja custom filters documentation`_.
`LOCALE` (''[#]_)                                                       更改时区. 提供区域列表或是代表区域的字符串。
`READERS` (``{}``)                                                      生成或者忽略文件扩展或者阅读分类字典。例如： 避免生成 .html 文件,
                                                                        set: ``READERS = {'html': None}``. 添加自定义 `foo` 扩展阅读，set: ``READERS = {'foo': FooReader}``
`IGNORE_FILES` (``['.#*']``)                                            文件匹配模式列表。用于忽略一些源文件。例如,
                                                                        默认 ``['.#*']`` 将会忽略emacs编辑器的锁定文件.
`MD_EXTENSIONS` (``['codehilite(css_class=highlight)','extra']``)       关于Markdown可用的扩展列表。 参考Python Markdown 文档查看支持扩展列表：
                                                                        `Extensions section <http://pythonhosted.org/Markdown/extensions/>`_ (注意：在设置文件中定义该值将会覆盖和替换默认值，如果你的目标是在默认值上添加设置，则需要明确说明并列举所需Marksown完整扩展列表。
`OUTPUT_PATH` (``'output/'``)                                           文件生成目录
`PATH` (``None``)                                                       输入文件目录
`PAGE_DIR` (``'pages'``)                                                页面生成目录
`PAGE_EXCLUDES` (``()``)                                                查找页面需要忽略的文件夹列表
`ARTICLE_DIR` (``''``)                                                  文章输入文件目录
`ARTICLE_EXCLUDES`: (``('pages',)``)                                    查找文章需要忽略的文件夹列表
`OUTPUT_SOURCES` (``False``)                                            如果希望复制文章和页面的原始格式(e.g. Markdown or reStructuredText)，请设置为True并指定 ``OUTPUT_PATH``.
`OUTPUT_SOURCES_EXTENSION` (``.text``)                                  控制资源生成扩展，默认值为 ``.text``. 如果没有有效的字符串，将使用默认值。
`RELATIVE_URLS` (``False``)                                             定义是否使用文档相对URL链接，只有当测试时设置为 ``True`` ，如果你完全理解该效果，则可以用于links或者feeds。
`PLUGINS` (``[]``)                                                      载入插件，参考： :ref:`plugins`.
`SITENAME` (``'A Pelican Blog'``)                                       站点名称
`SITEURL`                                                               站点URL。默认未定义，所以最好指定SITEURL；如果未指定，无法生成对应feeds的URLs。应该包含 ``http://`` 以及你的域名, 结尾不加/，例如: ``SITEURL = 'http://mydomain.com'``
`TEMPLATE_PAGES` (``None``)                                             包含文章生成时的模板页，请查看 :ref:`template_pages`.
`STATIC_PATHS` (``['images']``)                                         在output目录中提供可访问的静态路径 "static". 默认将会复制 "images" 文件夹到output目录.
`TIMEZONE`                                                              显示日期信息，生成Atom和RSS时间信息，查看 *Timezone* 章节
`TYPOGRIFY` (``False``)                                                 如果设置为True, 一些排版效果将会纳入生成的HTML文件中，通过 `Typogrify
                                                                        <http://static.mintchaos.com/projects/typogrify/>`_
                                                                        库, 安装方式: ``pip install typogrify``
`DIRECT_TEMPLATES` (``('index', 'tags', 'categories', 'archives')``)    直接使用模板。 通常情况下直接使用模板生成index页面的内容，(e.g., tags and
                                                                        category index pages). 如果无需标签和分类合集,设置 ``DIRECT_TEMPLATES = ('index', 'archives')``
`PAGINATED_DIRECT_TEMPLATES` (``('index',)``)                           提供可以分页的模板
`SUMMARY_MAX_LENGTH` (``50``)                                           文章摘要最大字数。如果设置为 ``None`` ，将导致摘要和原文内容一致.
`EXTRA_TEMPLATES_PATHS` (``[]``)                                        Jinja2 模板搜索列表.可以从主题中单独分离出来。
                                                                        例如: projects, resume, profile ...
                                                                        这些模板需要使用 ``DIRECT_TEMPLATES`` 设置.
`ASCIIDOC_OPTIONS` (``[]``)                                             将选项列表传递给AsciiDoc. 查看帮助文档 `manpage
                                                                        <http://www.methods.co.nz/asciidoc/manpage.html>`_
`WITH_FUTURE_DATES` (``True``)                                          如果仅用, 内容和日期将以草稿形式保存。
`INTRASITE_LINK_REGEX` (``'[{|](?P<what>.*?)[|}]'``)                    内部链接分析的正则表达式。默认内部链接语法标识符表明 ``filename``, 使用 ``{}`` 或者 ``||``. 标识符位于 ``{`` 和 ``}`` 之间.查看 :ref:`ref-linking-to-internal-content`.
`PYGMENTS_RST_OPTIONS` (``[]``)                                         针对reStructuredText代码块，Pygments默认设置列表，查看支持选项：:ref:`internal_pygments_options` 
=====================================================================   =====================================================================

.. [#] 默认为系统区域设置。


URL设置
-------

首先需要了解目前主要有两种方法支持URL格式：相对和绝对。本地测试的时候使用相对URLs十分方便，当站点发布时，使用绝对URLs更加可靠。两者兼得的一种方法则是设置Pelican配置文件，一个用作本地开发，另一个用来发布。查看此类设置的样例，请参考文档开始页面顶部的pelican-quickstart脚本，执行结果将会分别针对本地开发和远程发布产生两个单独的配置文件。

自定义URLs以及文件保存位置。``*_URL`` 和 ``*_SAVE_AS`` 变量使用python格式字符串。这些变量可以设置文章保存的位置，例如 ``{slug}/index.html`` ，这些设置可以灵活地将文章和页面保存在任意位置。

.. note::
    注意：如果你定义 ``datetime`` 指令，将会被文件中输入的date元数据属性所替换。如果没有指定date为特殊文件，Pelican将会根据该文件修改时间的时间戳。

参阅更多细节请参考python datetime文档：http://bit.ly/cNcJUC

当然，你也可以使用其它文件的元数据属性：

* slug
* date
* lang
* author
* category

用法示例:

* ARTICLE_URL = ``'posts/{date:%Y}/{date:%b}/{date:%d}/{slug}/'``
* ARTICLE_SAVE_AS = ``'posts/{date:%Y}/{date:%b}/{date:%d}/{slug}/index.html'``

以类似形式保存文章： ``/posts/2011/Aug/07/sample-post/index.html``,
URL链接形如： ``/posts/2011/Aug/07/sample-post/``.

Pelican可以选择按照年、月、日为文档归类。二级分类默认是禁用的，如果你提供对应的 ``_SAVE_AS`` 设置，将会自动启用。期间可以根据页面URLs的层次模型直观地归类，根据网页发布时间，便于读者进行阅读。

用法示例:

* YEAR_ARCHIVE_SAVE_AS = ``'posts/{date:%Y}/index.html'``
* MONTH_ARCHIVE_SAVE_AS = ``'posts/{date:%Y}/{date:%b}/index.html'``

With these settings, Pelican will create an archive of all your posts for the
year at (for instance) ``posts/2011/index.html`` and an archive of all your
posts for the month at ``posts/2011/Aug/index.html``.
按照以上方式设置，Pelican将会根据年份归类文章（例如） ``posts/2011/index.html`` ，如果以月份归档，则形如：  ``posts/2011/Aug/index.html``.

.. note::
    最终路径为 ``index.html`` 时，此时归类最佳。这样读者可以删除部分URL地址，自动到达适当的页面位置，而不必指定页面名称。

====================================================    =====================================================
名称设置 (默认值)                                         说明
====================================================    =====================================================
`ARTICLE_URL` (``'{slug}.html'``)                       文章URL
`ARTICLE_SAVE_AS` (``'{slug}.html'``)                   文章保存位置
`ARTICLE_LANG_URL` (``'{slug}-{lang}.html'``)           设置特定语言格式文章的URL
`ARTICLE_LANG_SAVE_AS` (``'{slug}-{lang}.html'``)       设置特定语言格式文章的存储位置
`PAGE_URL` (``'pages/{slug}.html'``)                    页面URL链接
`PAGE_SAVE_AS` (``'pages/{slug}.html'``)                页面存储位置
`PAGE_LANG_URL` (``'pages/{slug}-{lang}.html'``)        设置特定语言格式页面的URL
`PAGE_LANG_SAVE_AS` (``'pages/{slug}-{lang}.html'``)    设置特定语言格式页面的存储位置
`CATEGORY_URL` (``'category/{slug}.html'``)             类别URl
`CATEGORY_SAVE_AS` (``'category/{slug}.html'``)         类别保存位置
`TAG_URL` (``'tag/{slug}.html'``)                       标签URL
`TAG_SAVE_AS` (``'tag/{slug}.html'``)                   标签页面保存位置
`TAGS_URL` (``'tags.html'``)                            标签列表URL
`TAGS_SAVE_AS` (``'tags.html'``)                        标签列表保存位置
`AUTHOR_URL` (``'author/{slug}.html'``)                 作者URl
`AUTHOR_SAVE_AS` (``'author/{slug}.html'``)             作者存储位置
`AUTHORS_URL` (``'authors.html'``)                      作者列表URL
`AUTHORS_SAVE_AS` (``'authors.html'``)                  作者列表存储位置
`<DIRECT_TEMPLATE_NAME>_SAVE_AS`                        存储模板生成文件内容位置.  <DIRECT_TEMPLATE_NAME> 名称大写
`ARCHIVES_SAVE_AS` (``'archives.html'``)                文章归类页面存储位置
`YEAR_ARCHIVE_SAVE_AS` (False)                          按年归类的文章存储位置
`MONTH_ARCHIVE_SAVE_AS` (False)                         按月归类的文章存储位置
`DAY_ARCHIVE_SAVE_AS` (False)                           按日归类的文章存储位置
`SLUG_SUBSTITUTIONS`  (``()``)                          Substitutions to make prior to stripping out
                                                        non-alphanumerics when generating slugs. Specified
                                                        as a list of 2-tuples of ``(from, to)`` which are
                                                        applied in order.
====================================================    =====================================================

.. note::

    如果你不希望创建默认页面中的一个或多个（例如：在你的网站上你是唯一的作者，因此并不需要作者介绍页面），设置相应的 ``*_SAVE_AS`` 值为 ``None`` 。
    
时区
----

如果未定义时区，默认是UTC，这将意味着如果你的地理位置不是UTC（协调世界时——译者注），生成的Atom（ATOM是一种订阅网志的格式。一种Web feed，和RSS相类似。——译者注）以及RSS订阅将包含错误的日期信息。

倘若时区设置未定义，Pelican将会产生警告，在之前的版本中并未强制设置。

查看维基百科页面，获取有效的时区值列表： `the wikipedia page`_ 

.. _the wikipedia page: http://en.wikipedia.org/wiki/List_of_tz_database_time_zones


日期格式和语言环境
------------------

如果未设置 ``DATE_FORMATS`` ，Pelican将使用 ``DEFAULT_DATE_FORMAT`` ， 如果你需要保持多国语言与不同的日期格式，可以使用语言名称作为key设置 ``DATE_FORMATS`` 字典（在发布文章内容中的lang元数据中）。关于可用的格式代码，查看python的strftime文档： `strftime document of python`_ :

.. parsed-literal::

    DATE_FORMATS = {
        'en': '%a, %d %b %Y',
        'jp': '%Y-%m-%d(%a)',
    }

可以设置语言环境来进一步控制日期格式：

.. parsed-literal::

    LOCALE = ('usa', 'jpn',  # On Windows
        'en_US', 'ja_JP'     # On Unix/Linux
        )

可以为不同的语言设置不同的区域，如果再字典中使用 (locale, format) 元组，将会覆盖 ``LOCALE``
setting above:

.. parsed-literal::
    # On Unix/Linux
    DATE_FORMATS = {
        'en': ('en_US','%a, %d %b %Y'),
        'jp': ('ja_JP','%Y-%m-%d(%a)'),
    }

    # On Windows
    DATE_FORMATS = {
        'en': ('usa','%a, %d %b %Y'),
        'jp': ('jpn','%Y-%m-%d(%a)'),
    }

点击此处可以查看可用的windows语言环境列表：  `locales on Windows`_ ，在Unix/Linux系统上，通过 ``locale -a`` 命令可以获取可用的语言环境设置列表。参考locale手册获取更多信息： `locale(1)`_ 。


.. _strftime document of python: http://docs.python.org/library/datetime.html#strftime-strptime-behavior

.. _locales on Windows: http://msdn.microsoft.com/en-us/library/cdax410z%28VS.71%29.aspx

.. _locale(1): http://linux.die.net/man/1/locale


.. _template_pages:

模板页面
========

如果你希望生成除了博客条目以外的定制页面，可以设置任何 Jinja2 模板文件指向该页面的路径以及生成文件的目标路径。

例如，如果你的博客拥有三个静态页面——书目列表、个人简历，以及联系页面，你应该这样设置::

    TEMPLATE_PAGES = {'src/books.html': 'dest/books.html',
                      'src/resume.html': 'dest/resume.html',
                      'src/contact.html': 'dest/contact.html'}


.. _path_metadata:

路径元数据
==========

并非所有的元数据都需要嵌入在源文件中（ `embedded in source file itself`__），例如，博客名称经常被冠以如下命名形式： ``YYYY-MM-DD-SLUG.rst`` ，或者被嵌套到 ``YYYY/MM/DD-SLUG`` 目录。要提取文件名或路径的元数据，使用python的 `group name
notation`_ ``(?P<name>…)`` ，结合正则表达式设置 ``FILENAME_METADATA`` 或者 ``PATH_METADATA`` 属性值。如果你想添加额外的元数据，但是不希望在路径中进行转码，可以设置 ``EXTRA_PATH_METADATA`` ：

.. parsed-literal::

    EXTRA_PATH_METADATA = {
        'relative/path/to/file-1': {
            'key-1a': 'value-1a',
            'key-1b': 'value-1b',
            },
        'relative/path/to/file-2': {
            'key-2': 'value-2',
            },
        }


这便于移动特定文件的安装位置：

.. parsed-literal::

    # Take advantage of the following defaults
    # STATIC_SAVE_AS = '{path}'
    # STATIC_URL = '{path}'
    STATIC_PATHS = [
        'extra/robots.txt',
        ]
    EXTRA_PATH_METADATA = {
        'extra/robots.txt': {'path': 'robots.txt'},
        }

__ internal_metadata__
.. _group name notation:
   http://docs.python.org/3/library/re.html#regular-expression-syntax

Feed 设置
=========

默认情况下，Pelican使用Atom feeds，但是你也可以使用RSS feeds。

Pelican可以生成所有文章的分类feeds，并不针对默认的tags生成feeds，如果使用 ``TAG_FEED_ATOM`` 以及 ``TAG_FEED_RSS`` 设置可以实现：

================================================    =====================================================
名称设置 (默认值)                                     说明
================================================    =====================================================
`FEED_DOMAIN` (``None``, i.e. base URL is "/")      The domain prepended to feed URLs. Since feed URLs
                                                    should always be absolute, it is highly recommended
                                                    to define this (e.g., "http://feeds.example.com"). If
                                                    you have already explicitly defined SITEURL (see
                                                    above) and want to use the same domain for your
                                                    feeds, you can just set:  ``FEED_DOMAIN = SITEURL``.
`FEED_ATOM` (``None``, i.e. no Atom feed)           Relative URL to output the Atom feed.
`FEED_RSS` (``None``, i.e. no RSS)                  Relative URL to output the RSS feed.
`FEED_ALL_ATOM` (``'feeds/all.atom.xml'``)          Relative URL to output the all posts Atom feed:
                                                    this feed will contain all posts regardless of their
                                                    language.
`FEED_ALL_RSS` (``None``, i.e. no all RSS)          Relative URL to output the all posts RSS feed:
                                                    this feed will contain all posts regardless of their
                                                    language.
`CATEGORY_FEED_ATOM` ('feeds/%s.atom.xml'[2]_)      Where to put the category Atom feeds.
`CATEGORY_FEED_RSS` (``None``, i.e. no RSS)         Where to put the category RSS feeds.
`TAG_FEED_ATOM` (``None``, i.e. no tag feed)        Relative URL to output the tag Atom feed. It should
                                                    be defined using a "%s" match in the tag name.
`TAG_FEED_RSS` (``None``, ie no RSS tag feed)       Relative URL to output the tag RSS feed
`FEED_MAX_ITEMS`                                    Maximum number of items allowed in a feed. Feed item
                                                    quantity is unrestricted by default.
================================================    =====================================================

如果不希望生成任何feeds，将变量设置为 ``None``.

.. [2] %s is the name of the category.

FeedBurner
----------

如果你想在feed中使用FeedBurner，你可能需要确定一个唯一的标识符。例如，如果你的网站叫做 "Thyme" ，部署在www.example.com主机上，你可以使用“thymefeeds”作为你的唯一标识符，我们将在本节说明用途。在Pelican设置中，根据“thymefeeds/main.xml”页面设置 `FEED_ATOM` ，创建原始地址（http://www.example.com/thymefeeds/main.xml.）的Atom feed信息。如果你在自己的域名上使用CNAME（例如FeedBurner’s “MyBrand” feature），请根据 `http://feeds.feedburner.com`, 或者  `http://feeds.feedburner.com` 域名设置 `FEED_DOMAIN` 。

在FeedBurner接口中有两个字段可供配置：  `FeedBurner
<http://feedburner.google.com>`_ : "Original Feed" 和 "Feed
Address". 在本例中,  "Original Feed" 为
`http://www.example.com/thymefeeds/main.xml` 、 "Feed Address" 以 `thymefeeds/main.xml` 结尾.

分页
====

Pelican默认在首页上列出所有文章的标题并辅以简短的描述,这种技术适用于中小型网站，尤其是包含大量文章的网站。

设置分页配置：

================================================    =====================================================
名称设置 (默认值)                                     说明
================================================    =====================================================
`DEFAULT_ORPHANS` (``0``)                           The minimum number of articles allowed on the
                                                    last page. Use this when you don't want the last page
                                                    to only contain a handful of articles.
`DEFAULT_PAGINATION` (``False``)                    The maximum number of articles to include on a
                                                    page, not including orphans. False to disable
                                                    pagination.
`PAGINATION_PATTERNS`                               A set of patterns that are used to determine advanced
                                                    pagination output.
================================================    =====================================================

使用分页模式
------------

 ``PAGINATION_PATTERNS`` 用来配置创建的子页面，该设置是同序列的三元组，每个元组内容如下::

  (minimum page, URL setting, SAVE_AS setting,)

例如，如果你想第一页只是 ``/``，第二页（子页面）显示为 ``/page/2/``，你应该按照以下方式设置``PAGINATION_PATTERNS`` ::

  PAGINATION_PATTERNS = (
      (1, '{base_name}/', '{base_name}/index.html'),
      (2, '{base_name}/page/{number}/', '{base_name}/page/{number}/index.html'),
  )

This would cause the first page to be written to
``{base_name}/index.html``, and subsequent ones would be written into
``page/{number}`` directories.
首页以 ``{base_name}/index.html`` 形式生成，二级页面将保存在 ``page/{number}`` 目录下。

云标签
======

如果你想为所有的tags生成云标签，请使用如下设置：

================================================    =====================================================
名称设置 (默认值)                                    说明
================================================    =====================================================
`TAG_CLOUD_STEPS` (``4``)                           Count of different font sizes in the tag
                                                    cloud.
`TAG_CLOUD_MAX_ITEMS` (``100``)                     Maximum number of tags in the cloud.
================================================    =====================================================

默认主题未包含云标签，但是可以添加::

    <ul class="tagcloud">
        {% for tag in tag_cloud %}
            <li class="tag-{{ tag.1 }}"><a href="{{ SITEURL }}/{{ tag.0.url }}">{{ tag.0 }}</a></li>
        {% endfor %}
    </ul>

你也可以使用适当的类定义CSS样式（tag-0到tag-N，N匹配 ``TAG_CLOUD_STEPS`` -1），tag-0是最常见的，使用合适的list-style定义一个 ``ul.tagcloud`` 类，创建云。例如::

    ul.tagcloud {
      list-style: none;
        padding: 0;
    }

    ul.tagcloud li {
        display: inline-block;
    }

    li.tag-0 {
        font-size: 150%;
    }

    li.tag-1 {
        font-size: 120%;
    }

    ...    

翻译
====

Pelican提供一种翻译文章的方法，请参考第一节内容查阅：  :doc:`Getting Started <getting_started>` 。

=====================================================    =====================================================
名称设置 (默认值)                                         说明
=====================================================    =====================================================
`DEFAULT_LANG` (``'en'``)                                The default language to use.
`TRANSLATION_FEED_ATOM` ('feeds/all-%s.atom.xml'[3]_)    Where to put the Atom feed for translations.
`TRANSLATION_FEED_RSS` (``None``, i.e. no RSS)           Where to put the RSS feed for translations.
=====================================================    =====================================================

.. [3] %s is the language

内容排序
========

================================================    =====================================================
名称设置 (默认值)                                     说明
================================================    =====================================================
`NEWEST_FIRST_ARCHIVES` (``True``)                  Order archives by newest first by date. (False:
                                                    orders by date with older articles first.)
`REVERSE_CATEGORY_ORDER` (``False``)                Reverse the category order. (True: lists by reverse
                                                    alphabetical order; default lists alphabetically.)
================================================    =====================================================

主题
====

创建Pelican主题可以参考 (see :ref:`theming-pelican`)，以下是主题相关的设置：

================================================    =====================================================
名称设置 (默认值)                                      说明
================================================    =====================================================
`THEME`                                             Theme to use to produce the output. Can be a relative
                                                    or absolute path to a theme folder, or the name of a
                                                    default theme or a theme installed via
                                                    ``pelican-themes`` (see below).
`THEME_STATIC_DIR` (``'theme'``)                    Destination directory in the output path where
                                                    Pelican will place the files collected from
                                                    `THEME_STATIC_PATHS`. Default is `theme`.
`THEME_STATIC_PATHS` (``['static']``)               Static theme paths you want to copy. Default
                                                    value is `static`, but if your theme has
                                                    other static paths, you can put them here. If files
                                                    or directories with the same names are included in
                                                    the paths defined in this settings, they will be
                                                    progressively overwritten.
`CSS_FILE` (``'main.css'``)                         Specify the CSS file you want to load.
================================================    =====================================================


默认情况下，可用主题有两个，你可以根据主题设置进行定制，或者在pelican命令中输入 ``-t`` 选项::

* notmyidea
* simple (a synonym for "plain text" :)

可以浏览 http://github.com/getpelican/pelican-themes 获取更多可用主题，Pelican带有 :doc:`pelican-themes` （一个管理主题的小脚本）

你可以自定义主题，从头开始制作或者复制修改现有主题，请参阅主题创建指南:  :doc:`a guide on how to create your theme <themes>` 。

以下事例告诉你如何定义自己喜欢的主题 ::

    # Specify name of a built-in theme
    THEME = "notmyidea"
    # Specify name of a theme installed via the pelican-themes tool
    THEME = "chunk"
    # Specify a customized theme, via path relative to the settings file
    THEME = "themes/mycustomtheme"
    # Specify a customized theme, via absolute path
    THEME = "~/projects/mysite/themes/mycustomtheme"

按照如下设置使用内置 ``notmyidea`` 主题：

=======================   =======================================================
设置名称                     说明
=======================   =======================================================
`SITESUBTITLE`            A subtitle to appear in the header.
`DISQUS_SITENAME`         Pelican can handle Disqus comments. Specify the
                          Disqus sitename identifier here.
`GITHUB_URL`              Your GitHub URL (if you have one). It will then
                          use this information to create a GitHub ribbon.
`GOOGLE_ANALYTICS`        'UA-XXXX-YYYY' to activate Google Analytics.
`GOSQUARED_SITENAME`      'XXX-YYYYYY-X' to activate GoSquared.
`MENUITEMS`               A list of tuples (Title, URL) for additional menu
                          items to appear at the beginning of the main menu.
`PIWIK_URL`               URL to your Piwik server - without 'http://' at the
                          beginning.
`PIWIK_SSL_URL`           If the SSL-URL differs from the normal Piwik-URL
                          you have to include this setting too. (optional)
`PIWIK_SITE_ID`           ID for the monitored website. You can find the ID
                          in the Piwik admin interface > settings > websites.
`LINKS`                   A list of tuples (Title, URL) for links to appear on
                          the header.
`SOCIAL`                  A list of tuples (Title, URL) to appear in the
                          "social" section.
`TWITTER_USERNAME`        Allows for adding a button to articles to encourage
                          others to tweet about them. Add your Twitter username
                          if you want this button to appear.
=======================   =======================================================

此外，你还可以通过添加以下配置使用 "wide" 版本的 ``notmyidea`` 主题::

    CSS_FILE = "wide.css"

样例设置
========

.. literalinclude:: ../samples/pelican.conf.py
    :language: python


.. _Jinja custom filters documentation: http://jinja.pocoo.org/docs/api/#custom-filters
