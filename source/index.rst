Pelican
=======

Pelican是一种静态网站生成器，使用Python_语言编写。

* 使用您喜欢的编辑器（Vim!）直接编写内容，支持 reStructuredText_ , Markdown_ , 或者 AsciiDoc_ 等格式。
* 使用简单的命令行工具（重新）生成站点内容。
* 易于结合版本控制系统及其web应用搭配使用。
* 全静态化输出，易于部署。

特性
----

Pelican当前版本支持以下特性：

* 文章类（例如博客网站）和单页类（例如："About", "Projects", "Contact"等页面）
* 通过扩展支持评论功能（ Disqus_ ）（请注意Disqus是一种第三方服务，所有的评论数据在你控制之外，可能会面临潜在的数据丢失等风险。）
* 主题支持（使用 Jinja2_ 模板创建主题）
* 支持多种语言发布文章
* Atom/RSS 输出
* 语法高亮
* 导入WordPress, Dotclear, 或者RSS源数据
* 集成第三方工具: Twitter, Google Analytics 等（可选）

“Pelican”命名由来
-------------------

“Pelican”是由单词 *calepin* 逆序而来，在法语中表示“笔记”。；）

源码
----

您可以通过 https://github.com/getpelican/pelican 获取源码。

反馈/联系我们
-------------

如果你想Pelican具备新的特性，请不吝赐教，可以通过克隆代码仓库进行修改并提供建议。:doc:`参与建设<contribute>` 的方式有很多种，因为它是开源的，小伙伴。

有任何建议和意见请发邮件至：authors@getpelican.com
为了得到快速回答，你可以通过IRC频道 `#pelican on Freenode`_ 加入该团队——如果你身边没有得心应手的IRC聊天客户端，请使用 webchat_ 进行快速反馈。如果你通过IRC提出一个问题，并未快速得到回应，请不要离开该频道！由于时区不同，可能需要几个小时，如果你足够耐心，依然保持在线，会有人持续对你的询问进行回应。

文档
----

法语版的使用说明文档在此查看: Pelican_docs_French_

.. toctree::
   :maxdepth: 2

   getting_started
   settings
   themes
   plugins
   internals
   pelican-themes
   importer
   faq
   tips
   contribute
   report
   changelog

.. Links

.. _Python: http://www.python.org/
.. _reStructuredText: http://docutils.sourceforge.net/rst.html
.. _Markdown: http://daringfireball.net/projects/markdown/
.. _AsciiDoc: http://www.methods.co.nz/asciidoc/index.html
.. _Jinja2: http://jinja.pocoo.org/
.. _`Pelican documentation`: http://docs.getpelican.com/latest/
.. _`Pelican's internals`: http://docs.getpelican.com/en/latest/internals.html
.. _`#pelican on Freenode`: irc://irc.freenode.net/pelican
.. _webchat: http://webchat.freenode.net/?channels=pelican&uio=d4
.. _Pelican_docs_French: http://docs.getpelican.com/en/3.3.0/fr/index.html
.. _Disqus: https://disqus.com/
