pelican-themes
##############



说明
====

``pelican-themes`` 是一个用于管理主题的命令行工具。


用法
""""

| pelican-themes [-h] [-l] [-i theme path [theme path ...]]
|                      [-r theme name [theme name ...]]
|                      [-s theme path [theme path ...]] [-v] [--version]

可选参数:
"""""""""


-h, --help                              Show the help an exit

-l, --list                              Show the themes already installed

-i theme_path, --install theme_path     One or more themes to install

-r theme_name, --remove theme_name      One or more themes to remove

-s theme_path, --symlink theme_path     Same as "--install", but create a symbolic link instead of copying the theme.
                                        Useful for theme development

-v, --verbose                           Verbose output

--version                               Print the version of this script



实例
====


列出所有已经安装的主题
""""""""""""""""""""""

通过 ``pelican-themes`` 命令选项 ``-l`` or ``--list`` , 可以查看所有可用主题 :

.. code-block:: console

    $ pelican-themes -l
    notmyidea
    two-column@
    simple
    $ pelican-themes --list
    notmyidea
    two-column@
    simple

在该实例中，可以发现3个可用主题: ``notmyidea``, ``simple``, 和 ``two-column``.

``two-column`` 主题使用@结尾，是因为该主题并未复制到Pelican theme路径中， 只是链接，(查看 `创建符合链接`_ 获取关于创建符号链接相关信息).

注意：可以通过 ``-v`` 或者 ``--verbose`` 结合 ``--list`` 选项，获取更详细的输出：

.. code-block:: console

    $ pelican-themes -v -l
    /usr/local/lib/python2.6/dist-packages/pelican-2.6.0-py2.6.egg/pelican/themes/notmyidea
    /usr/local/lib/python2.6/dist-packages/pelican-2.6.0-py2.6.egg/pelican/themes/two-column (symbolic link to `/home/skami/Dev/Python/pelican-themes/two-column')
    /usr/local/lib/python2.6/dist-packages/pelican-2.6.0-py2.6.egg/pelican/themes/simple


安装主题
""""""""

可以使用 ``-i`` 或者 ``--install`` 选项安装主题。该选项接受主题安装路径参数，可以与verbose选项相结合：

.. code-block:: console

    # pelican-themes --install ~/Dev/Python/pelican-themes/notmyidea-cms --verbose

.. code-block:: console

    # pelican-themes --install ~/Dev/Python/pelican-themes/notmyidea-cms\
                               ~/Dev/Python/pelican-themes/martyalchin \
                               --verbose

.. code-block:: console

    # pelican-themes -vi ~/Dev/Python/pelican-themes/two-column


卸载主题
""""""""

 ``pelican-themes`` 命令也可以用于卸载主题，使用 ``-r`` 或者 ``--remove`` 选项附加卸载的主题名称即可，可以结合 ``--verbose`` 选项使用。

.. code-block:: console

    # pelican-themes --remove two-column

.. code-block:: console

    # pelican-themes -r martyachin notmyidea-cmd -v





创建符合链接
""""""""""""

``pelican-themes`` 命令可以无需复制整个主题到Pelican themes路径，而是直接通过创建符号链接安装主题。

使用 ``-s`` or ``--symlink`` 创建符号链接到一个主题，它的工作原理和 ``--install`` 选项一致：

.. code-block:: console

    # pelican-themes --symlink ~/Dev/Python/pelican-themes/two-column

在该实例中， ``two-column`` 主题通过符号链接到Pelican themes path, 因此我们可以使用该主题，同时我们也可以修改而无须每次更改之后重新安装。

这对于主题开发十分有用:

.. code-block:: console

    $ sudo pelican-themes -s ~/Dev/Python/pelican-themes/two-column
    $ pelican ~/Blog/content -o /tmp/out -t two-column
    $ firefox /tmp/out/index.html
    $ vim ~/Dev/Pelican/pelican-themes/two-coumn/static/css/main.css
    $ pelican ~/Blog/content -o /tmp/out -t two-column
    $ cp /tmp/bg.png ~/Dev/Pelican/pelican-themes/two-coumn/static/img/bg.png
    $ pelican ~/Blog/content -o /tmp/out -t two-column
    $ vim ~/Dev/Pelican/pelican-themes/two-coumn/templates/index.html
    $ pelican ~/Blog/content -o /tmp/out -t two-column



同时执行多种任务
""""""""""""""""

 ``--install``, ``--remove`` 和 ``--symlink`` 选项不是互斥的，因此在同一命令行结合以上选项可以同时执行多项任务：

.. code-block:: console

    # pelican-themes --remove notmyidea-cms two-column \
                     --install ~/Dev/Python/pelican-themes/notmyidea-cms-fr \
                     --symlink ~/Dev/Python/pelican-themes/two-column \
                     --verbose

在该实例中，将会使用 ``notmyidea-cms-fr`` 主题替换 ``notmyidea-cms`` 主题



参阅
====

-   http://docs.notmyidea.org/alexis/pelican/
-   ``/usr/share/doc/pelican/`` if you have installed Pelican using the `APT repository <http://skami18.github.com/pelican-packages/>`_
