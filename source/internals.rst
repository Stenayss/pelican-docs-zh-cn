Pelican内部原理
###############

本节主要描述Pelican内部工作原理，如你所知，十分简单，但是稍作说明，无可厚非。  :)

在 :doc:`report` 小节，提供作者撰写的软件设计信息报告摘要。

.. _report: :doc:`report`

整体结构
========

Pelican主要工作方式是获取文件列表并加工输出特定格式，通常情况下，输入文件为reStructuredText、Markdown、AsciiDoc等格式，输出目录为博客内容，但是输入和输出都可以自定义。

程序逻辑则是将整个过程分成不同的类别和概念：

* **Writers** 负责撰写 .html文件, RSS         feeds等。由于这些操作经常使用，对象一旦被创建，则会传递给生成器。

* **Readers** 负责解析各种格式文档 (目前支持AsciiDoc, HTML, Markdown 和
  reStructuredText). 提供文件，则会返回元数据 (author, tags, category, etc.) 及其内容 (HTML-formatted).

* **Generators** 产生不同的输出。例如, Pelican 自带
  ``ArticlesGenerator`` 和 ``PageGenerator``. 提供配置，则会按需生成，通常则是从输入目录生成文件。

* 模板支持, 易于编写主题。语法为 `Jinja2 <http://jinja.pocoo.org/>`_ 易于掌握。

如何实现新的阅读解析功能？
==========================

您是否希望在Pelican中加入一种优秀的标记性语言？首要完成的任务则是创建 ``read`` 方法类，能够解析并返回HTML内容及其元数据。

Take a look at the Markdown reader::

    class MarkdownReader(BaseReader):
        enabled = bool(Markdown)

        def read(self, source_path):
            """Parse content and metadata of markdown files"""
            text = pelican_open(source_path)
            md = Markdown(extensions = ['meta', 'codehilite'])
            content = md.convert(text)

            metadata = {}
            for name, value in md.Meta.items():
                name = name.lower()
                meta = self.process_metadata(name, value[0])
                metadata[name] = meta
            return content, metadata

Simple, isn't it?

如果新的解析器需要额外的Python库，应该在 ``try...except`` 块中导入。在reader类中，设置 ``enabled`` 类属性来标记是否导入成功。用户可以使用喜欢的标记方法而无需安装无用的模块。

如何实现新的生成器？
====================

生成器包含两个重要方法，无须同时创建，现有的将会被调用。

* ``generate_context``, that is called first, for all the generators.
  Do whatever you have to do, and update the global context if needed. This
  context is shared between all generators, and will be passed to the
  templates. For instance, the ``PageGenerator`` ``generate_context`` method
  finds all the pages, transforms them into objects, and populates the context
  with them. Be careful *not* to output anything using this context at this
  stage, as it is likely to change by the effect of other generators.

* ``generate_output`` is then called. And guess what is it made for? Oh,
  generating the output.  :) It's here that you may want to look at the context
  and call the methods of the ``writer`` object that is passed as the first
  argument of this function. In the ``PageGenerator`` example, this method will
  look at all the pages recorded in the global context and output a file on
  the disk (using the writer method ``write_file``) for each page encountered.
