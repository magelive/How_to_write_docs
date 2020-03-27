reStructuredtext格式详解
==============================

reStructuredText是一种reStructuredText是一种轻量级的文本标记语言，简单易读，所见即所得的文本标记语言。

详细的信息可参考： `Learn reStructureText <https://learn-rst.readthedocs.io/zh_CN/latest/index.html>`_ 和 `sphinx 使用手册 <https://zh-sphinx-doc.readthedocs.io/en/latest/index.html>`_

这里只做一些基本的介绍。


基本语法
----------------------

标题
^^^^^^^^^^^^^^^^^^^^^^^^

x 级标题分别对应 ``<hx>...</hx>.``

rst 中各级标题使用符号衬在文字下一行, 并且, 符号的数目应不少于文字数目. 对于中文等宽字符, 一个字符对应两个普通符号.

注意, rst 并不在意使用的符号类型, 只需要是 “相同符号衬托文字” 就会被解析为标题, 并根据符号的出现顺序与嵌套结构划分标题层级.

一般来讲, 会用以下符号来标注标题层级.


::

    一级标题
    ########

    二级标题
    ********

    三级标题
    ========

    四级标题
    --------

    五级标题
    ^^^^^^^^

    六级标题
    """"""""

    七级标题
    ''''''''''''

段落
^^^^^^^^^^

段落是reST文档中最基础的部分，段落通过一个或者多个空行分隔开。左侧必须对齐（没有空格，或者有相同多的空格）。

rst 中, 缩进也是控制段落的一个因素, 相同层级的段落, 缩进应当是一样的. 段落的缩进, 会影响渲染后文字的缩进.

内联标记
^^^^^^^^^^^^^^

标准的reST 内联标记相当简单:
    
    * 星号: ``*text*`` 是强调 (斜体)
    * 双星号: ``**text**`` 重点强调 (加粗)
    * 反引号: ````text```` 代码样式

.. note::
    
    如果星号或反引号出现在文本会对行内标记分隔符引起混淆，他们必须用一个反斜杠进行转义。
    另需要注意的是：
        * 不能相互嵌套
        * 内容前后不能由空白: 这样写``* text*`` 是错误的
        * 如果内容需要使用*. 使用反斜杠转义，如: ``this is \*one\* word``

reST 也允许自定义 “文本解释角色”, 这意味着可以以特定的方式解释文本. sphinx以此方式提供语义标记及参考索引，操作符为 ``:rolename:`content```.

标准reST 提供以下规则:

    * ``:durole:`emphasis``` – 写成 ``*emphasis*``
    * ``:durole:`strong``` – 写成 ``**strong**``
    * ``:durole:`literal``` – 写成 ````literal````
    * ``:durole:`subscript``` – 下标
    * ``:durole:`superscript``` – 上标
    * ``:durole:`title-reference``` – 书、期刊等材料的标题

详情请查看 `内联标记 <https://zh-sphinx-doc.readthedocs.io/en/latest/markup/inline.html#inline-markup>`_ 。


列表
^^^^^^^^^^^^

列表可以嵌套，但是需跟父列表使用空行分隔。无序列表可以使用 ``*`` ``-`` 等符号，有序列则是枚举编号后跟一个点，或使用 ``#`` 后加点使其自动编号。

有序列表
""""""""""""""

.. code-block:: c
    
    1. 列1
    #. 列2

        a. 缩进列a
        #. 缩进列b

生成的列表如下：

1. 列1
#. 列2

    a. 缩进列a
    #. 缩进列b


无序列表 
"""""""""""""""

::
    
    * 列1
    * 列2

        * 缩进列a
        * 缩进列b

生成的列表如下：

    * 列1
    * 列2

        * 缩进列a
        * 缩进列b


::

    | 列1
    | 列2

        | 缩进列a
        | 缩进列b
    
生成列表如下：

    | 列1
    | 列2

        | 缩进列a
        | 缩进列b


文字代码
^^^^^^^^^^^^

在段落的后面使用标记 ``::`` 引出. 代码块必须缩进(同段落，需要与周围文本以空行分隔):

如下段文字代码块：
::
    这是一段正常文本. 下一段是代码文字::

        它不需要特别处理，仅是
        缩进就可以了.

        它可以有多行.

    再是正常的文本段.

渲染后的结果如下：

这是一段正常文本. 下一段是代码文字::

   它不需要特别处理，仅是
   缩进就可以了.

   它可以有多行.

再是正常的文本段.


一个 :: 符号, 在之后空一行, 并缩进一级后编辑代码. 当缩进结束时, 代表代码块结束. 可以指定代码高亮模式, 默认是 Python 代码的高亮模式.

要指定高亮模式, 应使用 code-block 指令. code-block 可以指定其他属性, 例如 :linenos: 显示行号等。具体命令信息可查看 `sphinx 指令章节 <https://www.sphinx.org.cn/usage/restructuredtext/directives.html#directive-highlight>`_


如下c代码段：
    
::

    .. code-block:: c
        :linenos:
        
     int main() 
     {
        printf("hello");
        return 0;
     }
    
渲染后效果如下：

.. code-block:: c

    int main()
    {
        printf("hello");
        return 0;
    }


:: 标记的处理非常聪明:

    * 如果出现在段落本身中，那么整个段落将会从文档中删除（也就是说不会出现在生成的文档中）。
    * 如果它前面的空白，标记将被删除。
    * 如果它的前面非空白，标记会被单个冒号取代。

表格
^^^^^^^^^
rst文件支持两种表格， 一种是网格表格，可以自定义表格的边框. 如下:

::
    
    +------------------------+------------+----------+----------+
    | Header row, column 1   | Header 2   | Header 3 | Header 4 |
    | (header rows optional) |            |          |          |
    +========================+============+==========+==========+
    | body row 1, column 1   | column 2   | column 3 | column 4 |
    +------------------------+------------+----------+----------+
    | body row 2             | ...        | ...      |          |
    +------------------------+------------+----------+----------+

渲染后效果如下：

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+

另一种是简单表格，书写简单, 但有一些限制: 需要有多行，且第一列元素不能分行显示，如下:

::

    =====  =====  =======
    A      B      A and B
    =====  =====  =======
    False  False  False
    True   False  False
    False  True   False
    True   True   True
    =====  =====  =======

渲染后效果如下：

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

超链接
^^^^^^^^^

外部链接
"""""""""""

使用 ```链接文本 <链接地址URL>`_`` 进行行内网络链接。如果链接文本应该是Web地址，则根本不需要特殊标记，解析器会在普通文本中查找链接和邮件地址。

.. note::

    链接文本与链接地址URL的开头尖括号(<)间必须要有空格

如本文本参考 sphinx 使用手册，在引用外部链接时

::

    `sphinx 使用手册 <https://zh-sphinx-doc.readthedocs.io/en/latest/index.html>`_

其渲染后的结果如下：

    `sphinx 使用手册 <https://zh-sphinx-doc.readthedocs.io/en/latest/index.html>`_


另外，也可以把链接和标签分开，其格式为段落中使用 ```标签`_``， 然后再另起一行，设置标签的地址 ``.. _标签：地址`` 

如：
::
    
    这是 `sphinx rest文档`_
    
    .. _sphinx reset文档: http://www.pythondoc.com/sphinx/rest.html

其渲染后结果如下：

这是 `sphinx rest文档`_. 

.. _sphinx rest文档: http://www.pythondoc.com/sphinx/rest.html


如果文档中多个链接指向的其实是同一地址，可以简略点只写一次:

如：
::

    `Google`_ 就是 `搜索引擎`_
    
    .. _`Google`:
    .. _`搜索引擎`: https://zh-sphinx-doc.readthedocs.io/en/latest/index.html


其渲染后结果如下：
    
    `Google`_ 就是 `搜索引擎`_
    
    .. _`Google`:
    .. _`搜索引擎`: https://google.com

.. _内部链接_link:

内部链接
""""""""""""

内部链接也可使用 ```标签`_`` 的方式进行连接，文档中的每一个标题, 都会自动生成一个锚点, 可以直接使用标题文本进行链接，如 `内部链接`_ ：

::

    `内部链接`_

另外也可通过sphinx提供的特殊reST角色完成，为了支持对任何文档中任意位置的交叉引用，使用标准reST标签 ``:ref:`` 。为此，标签名称在整个文档中必须是唯一的。您可以通过两种方式引用标签:

    * 如果在标题之前直接放置标签，可以用 ``:ref:`label-name``` 引用它。
      如当前的章节内部链接 

    ::
       
        .. _内部链接_link:
        
        内部链接
        """"""""""""
        xxx
        xxx

        引用内部链接, see :ref:`内部链接_link`.

其渲染后结果如下：

        引用内部链接, see :ref:`内部链接_link`.

替换语法
^^^^^^^^^^^^^
替换语法中的文本, 会在渲染时自动被定义好的语句替换.

语法格式如下：
::

    |yufa|

    .. |yufa| replace:: 语法

如:
::
    
    |hi|, word 

    .. |hi| replace:: Hello

渲染后结果如下：

|hi|, word 

.. |hi| replace:: Hello
    

分割线
^^^^^^^^^^^^^^

同markdown中一致
``-------------------``

渲染效果：

------------------------------


图片
^^^^^^^^^^^^

使用 ``image`` 指令. 开头两个点, 空一格, 输入 ``image`` , 然后连用两个冒号 :: 再空一格, 输入到图片地址, 图片地址可以使用相对路径或绝对路径, 相对路径是相对于文档文件的。另外可在 ``image`` 下面添加属性, 所有属性和 HTML 中的图片属性是一样的。

sphinx中 ``image`` 指令使用方法如下：

::
    
    .. image:: 文件地址
        (options)

使用本地文件时，Sphinx将会自动将图像文件拷贝到输出目录中，文件地址若是URL时，会直接使用该URL来绘制图片。

如：
::
    .. image:: https://github.com/corkami/pics/raw/master/tracing/ocean.png
        :align: center
        :width: 236px
        :height: 100px

渲染后结果如下：

.. image:: https://github.com/corkami/pics/raw/master/tracing/ocean.png
    :align: center
    :width: 236px
    :height: 100px

尾注
^^^^^^^^^^^^^
尾注和链接用法类似. 源代码中尾注内容可以放在任何位置, 但是引用尾注处必须使用空格与其他文本分。

使用 ``[#name]_`` 标记脚注位置，并在 “Footnotes ” 标题后添加脚注主体在文档底部

使用 ``[#]`` 自动编号. 或者使用 ``[#name]`` 为特定尾注命名。

如：

.. code-block:: c
    
    尾注 [#]_
    .. [#] 或叫脚注， footnote. 

渲染后效果如下：
    
    尾注 [#]_

    .. [#] 或叫脚注， footnote.


数学公式
^^^^^^^^^^^^
reStructuredText 的数学公式书写通过指令（Directives） ``:math`` 完成。如需网页上显示的话，则和其它所有标记语言一样需要引入 MathJax 或 KaTex js 库。

如：

::

    .. math::
    
        \alpha _t(i) = P(O_1, O_2, \ldots  O_t, q_t = S_i \lambda )

渲染后效果如下：

.. math::
    
    \alpha _t(i) = P(O_1, O_2, \ldots  O_t, q_t = S_i \lambda )

行内数学公式则是通过 math role 实现的：

如：

::

    该圆的面积为 :math:`A_\text{c} = (\pi/4) d^2`.

渲染后效果如下：

    该圆的面积为 :math:`A_\text{c} = (\pi/4) d^2`.

.. note::

    使用数学公式时，需要使用 sphinx.ext.mathjax 库

引文
^^^^^^^^^^^^

如果给脚注指定标签，则被解析为引文（Citations）：

如：

::
    
    请参阅 [文档001]_

    .. [文档001] `从Markdown到restructuredtext <https://macplay.github.io/posts/cong-markdown-dao-restructuredtext>`_

渲染后效果如下：

    请参阅 [文档001]_

    .. [文档001] `从Markdown到restructuredtext <https://macplay.github.io/posts/cong-markdown-dao-restructuredtext>`_


注释
^^^^^^^^^^^

每个显式标记块都不是有效的标记结构，它被视为注释。

以 ``.. + 空格`` 开始，其后的数据不渲染在页面上，您可以在注释后开始后缩进文本以形成多行注释。

如：

::
    
    注释测试开始

    ..
        多行注释1
        多行注释2

    .. 单行注释
    注释测试结束

其渲染效果如下：


    注释测试开始

    ..
        多行注释1
        多行注释2

    .. 单行注释
    注释测试结束

指令
-----------------

指令（Directives）作为 reStructuredText 语言的一种扩展机制，允许快速添加新的文档结构而无需对底层语法进行更改。reStructuredText 开箱已经内置了一批常用指令，上文中使用的 raw 和 code 其实就是指令。指令的重要功能之一是可以添加选项以控制解析器对该元素的渲染方式，譬如让图片以两倍高宽居中进行展示：
