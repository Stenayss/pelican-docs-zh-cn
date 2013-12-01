小贴士
######

以下内容为一些有用的小贴士。

发布到GitHub
============

`GitHub Pages <https://help.github.com/categories/20/articles>`_ 提供一种简洁高效的发布Pelican页面的方法， There are `two types of GitHub
Pages <https://help.github.com/articles/user-organization-and-project-pages>`_:
*Project Pages* and *User Pages*. 两者都支持Pelican页面发布。

Project Pages
-------------

首先需要 *push*  ``output`` 目录中由Pelican生成的文件到Github仓库 ``gh-pages`` 。（你需要了解一些关于github的使用方法——译者注）

ghp-import命令可以简化该过程  `ghp-import <https://github.com/davisp/ghp-import>`_, 通过 ``easy_install`` or ``pip`` 安装。

例如，如果站点源目录包含在Github仓库中，并且以Project
Pages形式发布页面，可以按照如下步骤操作::

    $ pelican content -o output -s pelicanconf.py
    $ ghp-import output
    $ git push origin gh-pages

通过 ``ghp-import output`` 命令对本地 ``gh-pages`` 分支 ``output`` 目录中的内容进行更新（如果分支不存在，直接创建即可）。 ``git push origin gh-pages`` 命令主要功能则是更新并发布远程（Github仓库） ``gh-pages`` 分支页面内容。
 
.. note::

    通过 ``pelican-quickstart`` 命令安装时创建成的Makefile文件默认设置Pelican发布为 Project Pages， 如上所述。

User Pages
----------

首先需要 *push*  ``output`` 目录中由Pelican生成的文件到Github仓库 ``master`` 分支仓库，形如 ``<username>.github.io`` （其中username为你注册github填写的用户名——译者注）。

其次，使用 ``ghp-import``::

    $ pelican content -o output -s pelicanconf.py
    $ ghp-import output
    $ git push git@github.com:elemoine/elemoine.github.io.git gh-pages:master

通过 ``git push`` 更新Github远程仓库内容。

.. note::

    To publish your Pelican site as User Pages, feel free to adjust the
    ``github`` target of the Makefile.

友情提示
--------

Tip #1:

可以将多条命令添加到post-commit hook 中，便于每次更改提交，例如, 你可以添加如下命令到
``.git/hooks/post-commit``::

    pelican content -o output -s pelicanconf.py && ghp-import output && git push origin gh-pages

Tip #2:

To use a `custom domain
<https://help.github.com/articles/setting-up-a-custom-domain-with-pages>`_ with
GitHub Pages, you need to put the domain of your site (e.g.,
``blog.example.com``) inside a ``CNAME`` file at the root of your site. To do
this, create the ``content/extra/`` directory and add a ``CNAME`` file to it.
Then use the ``STATIC_PATHS`` setting to tell Pelican to copy this file to your
output directory. For example::

    STATIC_PATHS = ['images', 'extra/CNAME']
    EXTRA_PATH_METADATA = {'extra/CNAME': {'path': 'CNAME'},}

如何在页面中添加YouTube 或者 Vimeo 视频
=======================================

最直接的方法则是将这些视频网站的代码内嵌到源文件中。

还可以使用Pelcian插件 ``liquid_tags``,
``pelican_youtube``, 或者 ``pelican_vimeo`` 在页面内容中嵌入视频。

其次，类似reST和Markdown格式的标记型语言带有插件可以直接内嵌视频地址。You can use `reST video directive
<https://gist.github.com/dbrgn/2922648>`_ for reST or `mdx_video plugin
<https://github.com/italomaia/mdx-video>`_ for Markdown.
