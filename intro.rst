概述
=======================

本文档描述的是如何使用sphinx+doxygen+markdown+plantuml来写一份漂亮的开发文档。

sphinx简介
-------------------

sphinx官方的定义如下：
Sphinx is a tool that makes it easy to create intelligent and beautiful documentation, written by Georg Brandl and licensed under the BSD license.

它最初是为创建Python文档而创建的，现已支持多种语言。Sphinx使用reStructuredText作为标记语言.

官方定义的特点如下：

    * Output formats(输出格式多样): HTML (including Windows HTML Help), LaTeX (for printable PDF versions), ePub, Texinfo, manual pages, plain text
    * Extensive cross-references(交叉引用): semantic markup and automatic links for functions, classes, citations, glossary terms and similar pieces of information
    * Hierarchical structure(层次分明): easy definition of a document tree, with automatic links to siblings, parents and children
    * Automatic indices(自动索引): general index as well as a language-specific module indices
    * Code handling(代码突出): automatic highlighting using the Pygments highlighter
    * Extensions(扩展突出): automatic testing of code snippets, inclusion of docstrings from Python modules (API docs), and more
    * Contributed extensions: more than 50 extensions contributed by users in a second repository; most of them installable from PyPI

doxygen简介
-------------------

Doxygen 是可以根据代码注释生成文档的工具。
Doxygen 就是在您写代码注释时，按照一些它所制订的规则来写的话，它就可以帮您产生出漂亮的文档。
Doxygen 的使用可分为两大部分。首先是特定格式的代码注释，第二便是利用Doxygen的工具来产生文档。
Doxygen 要帮助您实现如下三个功能：
    
    #. 直接使用doxygen生成对应的文档（如html、pdf等）
    #. 使用doxygen提取代码结构，还可能通过生成图来可视化各种元素间的关系
    #. 使用doxygen生成常规文档

Doxygen 支持的代码处理语言包含：c/c++、Java、Python、Object-C、C#、PHP等，其输出的格式支持: html、xml、LateX、PDF、Unix Man Page等。


markdown简介
--------------
Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。
Markdown 语言在 2004 由约翰·格鲁伯（英语：John Gruber）创建。
Markdown 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。
Markdown 编写的文档后缀为 .md, .markdown。

plantuml简介
-----------------
PlantUML 可以帮助您使用简单易读的文本描述来绘制UML图， 它是一个开源项目。它支持快速绘制各种UML图：时序图、用例图、类图、活动图、组件图、状态图、对象图、部署图、定时图，同时还支持以下非UML图：Network、线框图形界面、架构图、规范和描述语言 (SDL)、Ditaa diagram、甘特图、思维导图、Work Breakdown Structure diagram、以 AsciiMath 或 JLaTeXMath 符号的数学公式、Entity Relationship diagramt等。

PlantUML通过简单直观的语言来定义这些示意图。

reStructuredText文件简介
----------------------------

reStructuredText是一种reStructuredText是一种轻量级的文本标记语言，简单易读，所见即所得的文本标记语言。

其一般保存的文件以.rst为后缀。在必要的时候，.rst文件可以被转化成PDF或者HTML格式，也可以有Sphinx转化为LaTex,man等格式，现在被广泛的用于程序的文档撰写。


相应工具安装
------------------

sphinx安装
^^^^^^^^^^^^^^

ubuntu
""""""""""
::

    $ apt-get install python3-sphinx

RHEL, CentOS
"""""""""""""""""
::
    
    $ yum install python-sphinx


MacOS
""""""""""""
Sphinx can be installed using Homebrew, MacPorts, or as part of a Python distribution such as Anaconda.

Homebrew
'''''''''''

::
    
    $ brew install sphinx-doc 

MacPorts
'''''''''

::

    $ sudo port install py38-sphinx 
    
To set up the executable paths, use the port select command

::
    
    $ sudo port select --set python python38
    $ sudo port select --set sphinx py38-sphinx

Anaconda
''''''''''

::

    $conda install sphinx


Others
""""""""""""""""

查看有没有安装Python， 使用 ``python --version`` ，判断是否安装了Python, 如果没有建议安装Python3。

安装完Python后使用PyPI安装 sphinx

    ::

        $ pip install -U sphinx

或者使用源码的方式安装

::

    $ git clone https://github.com/sphinx-doc/sphinx
    $ cd sphinx
    $ pip install .

or

::

    $ pip install git+https://github.com/sphinx-doc/sphinx



doxygen安装
^^^^^^^^^^^^^

Ubuntu 
"""""""""""""""""
::

    $ sudo apt install doxygen

RHEL, CentOS
""""""""""""""""
::

    $ yum install doxygen

MacOS
"""""""""""""""
::

    $brew install doxygen

或使用安装文件安装：http://doxygen.nl/files/Doxygen-1.8.17.dmg

Windows
"""""""""""""""
下载Doxygen <http://doxygen.nl/files/doxygen-1.8.17-setup.exe> 安装文件安装。


installation from source
"""""""""""""""""""""""""
::
    
    $ git clone https://github.com/doxygen/doxygen.git
    $ cd doxygen 
    $ mkdir build
    $ cd build
    $ cmake -G "Unix Makefiles" ..
    $ make && make install

sphinx 插件安装
^^^^^^^^^^^^^^^^^^^^^^^^^

breathe
"""""""""""""
该插件主要是协助sphinx读取doxygen中生成的xml，其官方定义如下：
This is an extension to reStructuredText and Sphinx to be able to read and render the Doxygen xml output.

::
    
    $ pip3 install breathe

sphinxcontrib-plantuml
""""""""""""""""""""""""
该插件主要是协助sphinx使用plantuml绘制UML图。

::
    
    $ pip3 install sphinxcontrib-plantuml

sphinxcontrib-markdown
"""""""""""""""""""""""""
该插件主要协助sphinx解析markdown文件

::

    $ pip3 install sphinxcontrib-markdown 


在sphinxcontrib中还有另一个markdown相关的插件recommonmark和sphinx-markdown-tables

::
    
    $ pip3 install recommonmark
    $ pip3 install sphinx-markdown-tables

sphinx 主题安装
"""""""""""""""""""""""""""
::

    pip install sphinx_rtd_theme

更多sphinx主题可看：https://sphinx-themes.org/

plantuml 和 Graphviz 安装
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Graphviz 是一个由AT&T实验室启动的开源工具包，用于绘制DOT语言脚本描述的图形。
Plantuml 和 Doxygen 均使用 graphviz 自动生成类之间和文件之间的调用关系图，如不需要此功能可不安装该工具包。

plantuml是基于java实现的，因此我们可以直接获取其Jar包：http://sourceforge.net/projects/plantuml/files/plantuml.1.2020.5.jar/download

Graphviz安装方式可查看其官网 https://www.graphviz.org/download/

.. note::
    
    需要注意的是plantuml在官网中说道，plantuml使用Graphviz 2.39和2.40无法很好的工作，因此推荐使用Graphviz 2.38版本。


markdown 支持安装
^^^^^^^^^^^^^^^^^^^^^^^^^
::

    pip install --upgrade myst-parser


myst-parser 配置
"""""""""""""""""""""

:: 

    vi conf.py

在conf.py中extensions中添加myst_parser支持

::
    
    extensions = ['xxx', 'xxxx', 'myst_parser']

同时如果有需要的话，可以添加相应后缀的解析:

::

    source_suffix = {
        '.rst': 'restructuredtext',
        '.txt': 'markdown',
        '.md': 'markdown',
        '.xx': 'xxxxxx'
    }

.. note::
     
    MyST-Parser requires Sphinx 2.1 or newer.
