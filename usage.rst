使用方法
================================

我们以当前项目为例，首先我们先创建一个 _source 目录，目录中有如下源码：

::

    .
    ├── c
    │   ├── test.c
    │   └── test.h
    └── java
        └── test.java


sphinx
^^^^^^^^^^^^^^^^^^^

然后我们在docs目录中运行sphinx-quickstart

::
    
    $ cd docs
    $ sphinx-quickstart

    Welcome to the Sphinx 2.4.4 quickstart utility.

    Please enter values for the following settings (just press Enter to
    accept a default value, if one is given in brackets).

    Selected root path: .

    You have two options for placing the build directory for Sphinx output.
    Either, you use a directory "_build" within the root path, or you separate
    "source" and "build" directories within the root path.
    > Separate source and build directories (y/n) [n]:
    
    The project name will occur in several places in the built documentation.
    > Project name: Test
    > Author name(s): Test
    > Project release []: v1.0

    If the documents are to be written in a language other than English,
    you can select a language here by its language code. Sphinx will then
    translate text that it generates into that language.

    For a list of supported codes, see
    https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language.
    > Project language [en]:

    Creating file ./conf.py.
    Creating file ./index.rst.
    Creating file ./Makefile.
    Creating file ./make.bat.

    Finished: An initial directory structure has been created.
    
    You should now populate your master file ./index.rst and create other documentation
    source files. Use the Makefile to build the docs, like so:
       make builder
       where "builder" is one of the supported builders, e.g. html, latex or linkcheck.


执行完命令后，在当前目录下会生成如下文件：

::
    
    .
    ├── Makefile
    ├── _build
    ├── _static
    ├── _templates
    ├── conf.py
    ├── index.rst
    └── make.bat

* Makefile 为通用的Make文件，在我们生成html或pdf时，在类unix系统上时，只直接运行make html之类的命令生成html格式文档

* _build上档是生成文档时所在的目录
 
* _static是静态文件存储目录

*  _templates 是模板存放的目录

* conf.py 是sphinx的配置文件

* index.rst为主框架文件

* make.bat 是在windows上编译的脚本

更改sphinx配置conf.py

::
    
    # 修改其支持中文
    language = 'zh_CN'

    # 添加插件
    # sphinx.ext.napoleon 支持numpy和google风格的docstrings
    # sphinx.ext.autodoc 这个扩展可以导入您正在记录的模块，并以半自动的方式从docstring中拉入文档。
    # breathe doxygen和sphinx交互桥梁
    # sphinxcontrib.plantuml plantuml解析
    # sphinx_markdown_tables markdown 解析
    extensions = ['sphinx.ext.napoleon',
            'sphinx.ext.autodoc',
            'breathe',
            'sphinxcontrib.plantuml',
            'sphinx_markdown_tables'
        ]
    
    # 添加markdown 解析
    from recommonmark.parser import CommonMarkParser
    source_parsers = {
    '.md': CommonMarkParser
    }
    
    # 添加解析文件后缀，即解析所有后缀的文件
    source_suffix = ['.rst', '.md']
    
    # 排除需要处理的文件
    exclude_patterns = ['_build', 'Thumbs.db', '.DS_Store']


    # 设置pygments style
    pygments_style = 'sphinx'

    # 设置主题
    html_theme = 'sphinx_rtd_theme'

    # 设置静态文件目录
    html_static_path = ['_static']

    # breathe 相关设置
    # breathe_projects 需要与doxygen中的一致， 其格式为project:output

    breathe_projects = {"Test": "./_build/xml"}
    breathe_default_project = "Test"
    breathe_domain_by_extension = {"h" : "c"}


Doxygen
^^^^^^^^^^^^^^

使用doxygen命令生成 doxygen 配置文件Doxyfile
:: 
    
    $ doxygen -g

修改Doxyfile中的关键信息：

::

    # 设定需要doxygen 解析源码目录
    INPUT = _source/ 

    # Project name 需要与sphinx中breathe中设置的一致
    PROJECT_NAME           = "Test"

    # 若使用的C代码，需要开启OPTIMIZE_OUTPUT_FOR_C
    OPTIMIZE_OUTPUT_FOR_C = YES

    # 若使用的Java代码，需要开启OPTIMIZE_OUTPUT_JAVA
    OPTIMIZE_OUTPUT_JAVA = YES

    # 设定输出格式
    # Doxygen和sphinx结合输出文档，因此只需要输出xml格式的文档即可
    GENERATE_HTML = NO
    GENERATE_LATEX = NO 
    GENERATE_XML = YES

    # 设定xml output 位置
    XML_OUTPUT = _build/xml


Makefile/make.bat
^^^^^^^^^^^^^^^^^^^^^^^^^^^

在Makefile中添加 doxygen 解析 doxyfile

::
    
    %: Makefile 
        @doxygen Doxyfile
        @$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

在make.bat中的 `%SPHINXBUILD%` 上面一行添加：

::
    
    doxygen Doxyfile


添加其它的rst和md文件
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

打开index.rst文件

在如下内容的后面添加相关文件名即可，不需要带后缀

::
    
    .. toctre::
    :maxdepth: 2
    :caption: Contents:

  
如添加intro.rst、usage.rst、reference.md等文件：

::
    
    .. toctre:: 
    :maxdepth: 2
    :caption: Contents:

    intro
    usage
    reference

使用plantuml绘制流程图
^^^^^^^^^^^^^^^^^^^^^^^^^^^

使用 `.. uml::` 来使用uml图如，

如：

::

    .. uml::
        
        a->b: a=>b
        b->a: b=>a


生成的图形如下：

.. uml::

    a->b: a=>b
    b->a: b=>a

引用源文件
^^^^^^^^^^^^^^^^^^^^^

在引用源文件中的内容时，我们可以根据doxygen的格式在源文件中写好注释，后doxygen可将对应的注释信息提取到xml中，注释格式可看后续的doxygen注释格式章节。

test.h
"""""""""""""""
.. doxygenfile:: test.h

test.java
"""""""""""""
.. doxygenfile:: test.java




