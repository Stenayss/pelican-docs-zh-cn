.. _plugins:

插件
####

自从Pelican 3.0版本开始，则支持插件功能，使用插件无须修改Pelican核心代码即可为其添加相关功能。

插件使用
========

载入插件，需在设置文件中指定，两种方法可以实现，第一种方法是指定可调用的字符串路径::

    PLUGINS = ['package.myplugin',]

另一种方法是将他们添加到如下列表中::

    from package import myplugin
    PLUGINS = [myplugin,]

如果你的插件不在导入路径之内，可以在设置文件中指定 ``PLUGIN_PATH`` 值， ``PLUGIN_PATH`` 可以在设置文件中定义为绝对路径或者相对路径::

    PLUGIN_PATH = "plugins"
    PLUGINS = ["list", "of", "plugins"]

插件下载
========

我们为使用者提供单独的插件库可供分享和使用，请前往 `pelican-plugins`_  代码库查看可用插件列表。

.. _pelican-plugins: https://github.com/getpelican/pelican-plugins

注意：这些插件大都是由Pelican社区提供，因此在支持和操作上存在不同程度的差异。

插件制作
========

插件是基于信号的概念，Pelican发出信号，插件接收这些信号，信号列表的定义请参考下一段内容。

唯一需要遵守的规则是定义一个可调用的寄存器 ``register`` ，在该寄存器中映射信号到插件中，让我们简单测试一下::

    from pelican import signals

    def test(sender):
        print "%s initialized !!" % sender

    def register():
        signals.initialized.connect(test)

信号列表
========

目前有效的信号列表如下：

=============================   ============================   ===========================================================================
信号                             参数                           说明
=============================   ============================   ===========================================================================
initialized                     pelican object
finalized                       pelican object                  invoked after all the generators are executed and just before pelican exits
                                                                usefull for custom post processing actions, such as:
                                                                - minifying js/css assets.
                                                                - notify/ping search engines with an updated sitemap.
generator_init                  generator                       invoked in the Generator.__init__
readers_init                    readers                         invoked in the Readers.__init__
article_generator_context        article_generator, metadata
article_generator_preread        article_generator              invoked before a article is read in ArticlesGenerator.generate_context;
                                                                use if code needs to do something before every article is parsed
article_generator_init          article_generator               invoked in the ArticlesGenerator.__init__
article_generator_finalized     article_generator               invoked at the end of ArticlesGenerator.generate_context
get_generators                  generators                      invoked in Pelican.get_generator_classes,
                                                                can return a Generator, or several
                                                                generator in a tuple or in a list.
page_generator_context          page_generator, metadata
page_generator_init             page_generator                  invoked in the PagesGenerator.__init__
page_generator_finalized        page_generator                  invoked at the end of PagesGenerator.generate_context
content_object_init             content_object                  invoked at the end of Content.__init__ (see note below)
content_written                 path, context                   invoked each time a content file is written.
=============================   ============================   ===========================================================================

这份列表目前并不完善，所以如果你需要它们，请尽管使用。

.. note::

    ``content_object_init`` 可以发送不同类型的对象作为参数，如果你希望只注册一种类型的对象，当进行信号连接时，需要指定 sender

   ::

       from pelican import signals
       from pelican import contents

       def test(sender, instance):
               print "%s : %s content initialized !!" % (sender, instance)

       def register():
               signals.content_object_init.connect(test, sender=contents.Article)

.. note::

   在Pelican 3.2版本之后，信号命名开始标准化，一些较老的插件需要更新并使用新的命名

   ==========================  ===========================
   Old name                    New name
   ==========================  ===========================
   article_generate_context    article_generator_context
   article_generate_finalized  article_generator_finalized
   article_generate_preread    article_generator_preread
   pages_generate_context      page_generator_context
   pages_generate_preread      page_generator_preread
   pages_generator_finalized   page_generator_finalized
   pages_generator_init        page_generator_init
   static_generate_context     static_generator_context
   static_generate_preread     static_generator_preread
   ==========================  ===========================

方法
====

我们最终总结了一些插件制作的方法，请查看分享文档：

创建阅读器
----------

有个问题你可能会想到，如何支持你自定义的输入格式，虽然在Pelican核心代码中增加这个功能十分有意义，但是我们依然避免出现这种情况，你可以通过插件定义不同的阅读器。

这一背后的理由主要是插件易于编写且不会降低Pelican自身运行速度。

多说无益，请看样例::

    from pelican import signals
    from pelican.readers import BaseReader

    # Create a new reader class, inheriting from the pelican.reader.BaseReader
    class NewReader(BaseReader):
        enabled = True  # Yeah, you probably want that :-)

        # The list of file extensions you want this reader to match with.
        # If multiple readers were to use the same extension, the latest will
        # win (so the one you're defining here, most probably).
        file_extensions = ['yeah']

        # You need to have a read method, which takes a filename and returns
        # some content and the associated metadata.
        def read(self, filename):
            metadata = {'title': 'Oh yeah',
                        'category': 'Foo',
                        'date': '2012-12-01'}

            parsed = {}
            for key, value in metadata.items():
                parsed[key] = self.process_metadata(key, value)

            return "Some content", parsed

    def add_reader(readers):
        readers.reader_classes['yeah'] = NewReader

    # This is how pelican works.
    def register():
        signals.readers_init.connect(add_reader)


增加新的生成器
--------------

添加新的生成器十分容易，你可能要看看关于如何创建自己专属生成器的相关信息 :doc:`internals` 。

::

    def get_generators(generators):
        # define a new generator here if you need to
        return generators

    signals.get_generators.connect(get_generators)
