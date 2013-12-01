.. _import:

========================
 从其它Blog软件导入数据
========================


说明
====

``pelican-import`` 是一种基于命令行的工具，负责将其它blog中的文章数据转换为reStructuredText 或 Markdown 格式，支持的转换格式为：

- WordPress XML export
- Dotclear export
- Posterous API
- Tumblr API
- RSS/Atom feed

将HTML格式转换为reStructuredText 或 Markdown格式，主要依赖 `Pandoc`_ ，关于Dotclear，如果源文件使用Markdown语法书写，则无需转换。

依赖包
======

``pelican-import`` 包括一些未安装的依赖包：

- *BeautifulSoup4* and *lxml*, 用于WordPress 和 Dotclear 转换. 安装方式和其它Python package安装方法类似 (``pip install BeautifulSoup4 lxml``).
- *Feedparser*, for feed import (``pip install feedparser``).
- *Pandoc*, see the `Pandoc site`_ for installation instructions on your
  operating system.

.. _Pandoc: http://johnmacfarlane.net/pandoc/
.. _Pandoc site: http://johnmacfarlane.net/pandoc/installing.html


用法
====

::

    pelican-import [-h] [--wpfile] [--dotclear] [--posterous] [--tumblr] [--feed] [-o OUTPUT]
                   [-m MARKUP] [--dir-cat] [--dir-page] [--strip-raw] [--disable-slugs]
                   [-e EMAIL] [-p PASSWORD] [-b BLOGNAME]
                   input|api_token|api_key

Positional arguments
--------------------

  input                 解析输入文件
  api_token             [Posterous only] api_token can be obtained from http://posterous.com/api/
  api_key               [Tumblr only] api_key can be obtained from http://www.tumblr.com/oauth/apps

可选参数
--------

  -h, --help            帮助和退出
  --wpfile              WordPress XML格式输出 (default: False)
  --dotclear            Dotclear 输出 (default: False)
  --posterous           Posterous API (default: False)
  --tumblr              Tumblr API (default: False)
  --feed                Feed to parse (default: False)
  -o OUTPUT, --output OUTPUT
                        输出路径 (default: output)
  -m MARKUP, --markup MARKUP
                        输出标记格式 (supports rst & markdown)
                        (default: rst)
  --dir-cat             基于类别名称的目录文件
                        (default: False)
  --dir-page            Put files recognised as pages in "pages/" sub-
                          directory (wordpress import only) (default: False)
  --filter-author       仅导入指定作者的文章内容
  --strip-raw           Strip raw HTML code that can't be converted to markup
                        such as flash embeds or iframes (wordpress import
                        only) (default: False)
  --disable-slugs       Disable storing slugs from imported posts within
                        output. With this disabled, your Pelican URLs may not
                        be consistent with your original posts. (default:
                        False)
  -e EMAIL, --email=EMAIL
                        Email used to authenticate Posterous API
  -p PASSWORD, --password=PASSWORD
                        Password used to authenticate Posterous API
  -b BLOGNAME, --blogname=BLOGNAME
                        Blog name used in Tumblr API


实例
====

For WordPress::

    $ pelican-import --wpfile -o ~/output ~/posts.xml

For Dotclear::

    $ pelican-import --dotclear -o ~/output ~/backup.txt

for Posterous::

    $ pelican-import --posterous -o ~/output --email=<email_address> --password=<password> <api_token>

For Tumblr::

    $ pelican-import --tumblr -o ~/output --blogname=<blogname> <api_token>

Tests
=====

To test the module, one can use sample files:

- for WordPress: http://wpcandy.com/made/the-sample-post-collection
- for Dotclear: http://themes.dotaddict.org/files/public/downloads/lorem-backup.txt
