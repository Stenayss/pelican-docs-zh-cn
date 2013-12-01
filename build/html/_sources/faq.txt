疑问解答 (FAQ)
################################

下面是关于Pelican常见问题解答.

卓有成效的交流方式？
======================================================================

如果有疑问、意见或建议，请联系 `#pelican on Freenode <irc://irc.freenode.net/pelican>`_. 如果没有IRC客户端请通过webchat与我们联系：`IRC webchat <http://webchat.freenode.net/?channels=pelican&uio=d4>`_. 受不同时区影响，信息可能无法及时到达，请保持耐心并持续在线——有人会进行解答（可能需要等待几个小时）

如果问题依然未被解决，请前往： `issue tracker <https://github.com/getpelican/pelican/issues>`_.

如何支持Pelican发展?
====================

方法多种多样，首先，可以通过IRC或者反馈问题或建议 `issue tracker
<https://github.com/getpelican/pelican/issues>`_. 如果想要提交问题报告，请首先检查问题列表，避免重复提交。

如果你希望参与Pelican改进，请fork `the git repository
<https://github.com/getpelican/pelican/>`_, 创建新的分支，提交修改、反馈问题，会有人及时回复你的问题。更多信息请参考： :doc:`How to Contribute <contribute>` 

还可以通过制作主题、完善参考文档等方式支持我们。

必须要有配置文件吗?
===================

配置文件是可选的，只是一种配置Pelican较为简便的方法，对于基础操作，通过命令行调用Pelican时，需要指定选项，查看更多信息： ``pelican --help`` 。

对于自建主题，如何利用Pygments的语法高亮效果？
==============================================

Pygments对于生成内容添加一些类，这些类通过CSS控制主题语法高亮。可以通过主题CSS文件中 ``.highlight pre`` 类进行语法高亮自定义。例如，查看关于Django代码不同风格的语法高亮，可以通过样式选择器右上方的下拉框进行选择： `Pygments project demo site <http://pygments.org/demo/>`_.

根据以下命令，通过Pygments内置样式生成CSS文件（本例中，名称为 "monokai"），拷贝该CSS到主题目录 ::

    pygmentize -S monokai -f html -a .highlight > pygment.css
    cp pygment.css path/to/theme/static/css/

记得导入 ``pygment.css`` 文件。

如何自制主题?
=============

查看 :ref:`theming-pelican`.

使用Markdown语法，显示 ``No valid files found in content`` 错误。
=================================================================

Markdown并非是Pelican唯一依赖的语法格式，因此关于Markdown格式问题，需要明确是否安装Markdown库。
通过以下命令安装markdown::

    pip install markdown

如果无法通过 ``pip`` 命令安装，需要先安装 ``pip`` 命令::

    easy_install pip

是否可以在模板中使用任意元数据？
================================

当然可以！例如，在Markdown文件中包含修改日期，可以在文章顶部加入如下内容::

    Modified: 2012-08-08

如果是reStructuredText语法格式，则::

    :Modified: 2012-08-08

通过以下if语句，元数据可以通过模板生成形如 ``article.html`` 文件::

    {% if article.modified %}
    Last modified: {{ article.modified }}
    {% endif %}

如果希望在文章内容之外包含元数据 (e.g.,
``base.html``),  ``if`` 语句应该如下::

    {% if article and article.modified %}

如何指定模板？
==============

直接在页面或者文章头部添加额外的元数据行，指定专用模板，例如（reST格式）::

    :template: template_name

Markdown 格式::

    Template: template_name

只需保证所使用的主题中包含相关的模板文件即可 (e.g.
``template_name.html``).

如何重定义特定页面或者文章的URL？
=================================

通过页面元数据： ``url`` 和 ``save_as`` 。以下是关于reST格式的实例::

    Override url/save_as page
    #########################

    :url: override/url/
    :save_as: override/url/index.html

根据以上元数据，重新生成的页面位置为 ``override/url/index.html`` Pelican 会使用 ``override/url/`` url链接到该页面。

如何使用某个静态页面作为我的首页？
==================================

以上重定义元数据可以指定某个静态页面作为首页， 以下Markdown内容将会存储在 ``content/pages/home.md``::

    Title: Welcome to My Site
    URL: 
    save_as: index.html

    Thank you for visiting. Welcome!

如何禁用生成feed？
==================

禁用此功能，所有的feed设置都应该设置为 ``None`` 。 其中3个feed默认设置为 ``None``, 因此只需设置以下3个feed为 ``None`` 即可::

    FEED_ALL_ATOM = None
    CATEGORY_FEED_ATOM = None
    TRANSLATION_FEED_ATOM = None

注意： ``None`` 和 ``''`` 并不是一回事， ``None`` 无需加引号。

当 SITEURL 设置不正确，生成feeds时产生警告
==========================================

`RSS and Atom feeds require all URL links to be absolute
<http://validator.w3.org/feed/docs/rss2.html#comments>`_.
RSS和Atom feeds需要绝对路径链接。为了正确生成链接，需要设置 ``SITEURL`` 为你的网站完整路径。

当显示该错误时，Feeds依然会生成，但是相关链接可能会出问题，并且feed可能无法正常使用。

升级到Pelican 3.x，feeds文件损坏？
==================================

从3.0版本开始，部分FEED设置名称发生变化，更加接近Atom feeds本质内容， (接近 FEED_RSS
设置命名). 设置名称变化内容如下::

    FEED -> FEED_ATOM
    TAG_FEED -> TAG_FEED_ATOM
    CATEGORY_FEED -> CATEGORY_FEED_ATOM

从3.1版本开始，引入 ``FEED_ALL_ATOM`` : 该feed将所有语言相关的文章都聚合在一起，默认设置生成 ``'feeds/all.atom.xml'`` ， ``FEED_ATOM`` 现在默认设置为 ``None``. 以下feed设置已经重新命名::

    TRANSLATION_FEED -> TRANSLATION_FEED_ATOM

老版本主题及其设置可能会存在链接错误。为了解决此问题，请更新主题，并更改模板文件中相关值。具体实例和用法请参考 ``simple`` 主题。

Pelican只是适合生成博客吗？
===========================

当然不是！Pelican可以十分方便生成任何类型的静态页面。需要进行简单定义和配置，例如，如果你希望为你的产品建立宣传页面，不需要设置站点标签，可以在主题中删除相关HTML代码，或者可以禁用所有tag相关的选项::

    TAGS_SAVE_AS = ''
    TAG_SAVE_AS = ''
