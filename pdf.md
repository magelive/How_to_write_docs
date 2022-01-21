# 使用rst2pdf拓展sphinx生成PDF

## rst2pdf介绍

rst2pdf是一个将 reStructuredText 转换为 PDF 的工具，具有下列特性：

* 自定义页面布局

* 支持层叠样式表

* 支持内嵌TTF和Type1字体

* 支持几乎所有语言的语法高亮

* 使用reStructuredText作为源文件

* 支持字间距调整

## 安装rst2pdf

```shell
pip3 install rst2pdf
```

## 配置rst2pdf 

需要告诉sphinx我们安装了rst2pdf，并且将其作为插件使用。只需在项目根目录下的conf.py中配置：

```python
extensions = [
    ...,
    'rst2pdf.pdfbuilder'
]
```

然后，在conf.py中拷入PDF相关的配置：

```python
# -- Options for PDF output --------------------------------------------------
 
# Grouping the document tree into PDF files. List of tuples
# (source start file, target name, title, author, options).
#
# If there is more than one author, separate them with \\.
# For example: r'Guido van Rossum\\Fred L. Drake, Jr., editor'
#
# The options element is a dictionary that lets you override
# this config per-document.
# For example,
# ('index', u'MyProject', u'My Project', u'Author Name',
#  dict(pdf_compressed = True))
# would mean that specific document would be compressed
# regardless of the global pdf_compressed setting.
 
pdf_documents = [
    ('index', u'HanLP Handbook', u'HanLP Handbook', u'hankcs'),
]
 
# A comma-separated list of custom stylesheets. Example:
pdf_stylesheets = ['a3','zh_CN']
 
# Create a compressed PDF
# Use True/False or 1/0
# Example: compressed=True
#pdf_compressed = False
 
# A colon-separated list of folders to search for fonts. Example:
pdf_font_path = ['C:\\Windows\\Fonts']
 
# Language to be used for hyphenation support
pdf_language = "zh_CN"
 
# Mode for literal blocks wider than the frame. Can be
# overflow, shrink or truncate
pdf_fit_mode = "shrink"
 
# Section level that forces a break page.
# For example: 1 means top-level sections start in a new page
# 0 means disabled
#pdf_break_level = 0
 
# When a section starts in a new page, force it to be 'even', 'odd',
# or just use 'any'
#pdf_breakside = 'any'
 
# Insert footnotes where they are defined instead of
# at the end.
#pdf_inline_footnotes = True
 
# verbosity level. 0 1 or 2
#pdf_verbosity = 0
 
# If false, no index is generated.
#pdf_use_index = True
 
# If false, no modindex is generated.
#pdf_use_modindex = True
 
# If false, no coverpage is generated.
#pdf_use_coverpage = True
 
# Documents to append as an appendix to all manuals.
#pdf_appendices = []
 
# Enable experimental feature to split table cells. Use it
# if you get "DelayedTable too big" errors
#pdf_splittables = False
 
# Set the default DPI for images
#pdf_default_dpi = 72
 
# Enable rst2pdf extension modules (default is only vectorpdf)
# you need vectorpdf if you want to use sphinx's graphviz support
#pdf_extensions = ['vectorpdf']
 
# Page template name for "regular" pages
#pdf_page_template = 'cutePage'
 
# Show Table Of Contents at the beginning?
# pdf_use_toc = False
 
# How many levels deep should the table of contents be?
pdf_toc_depth = 2
 
# Add section number to section references
pdf_use_numbered_links = False
 
# Background images fitting mode
pdf_fit_background_mode = 'scale'
```

在项目根目录下创建一个zh_CN.json样式表，写入：

```json
{
  "embeddedFonts": [
    "simsun.ttc"
  ],
  "fontsAlias": {
    "stdFont": "simsun",
    "stdBold": "simsun",
    "stdItalic": "simsun",
    "stdBoldItalic": "simsun",
    "stdMono": "simsun",
    "stdMonoBold": "simsun",
    "stdMonoItalic": "simsun",
    "stdMonoBoldItalic": "simsun",
    "stdSans": "simsun",
    "stdSansBold": "simsun",
    "stdSansItalic": "simsun",
    "stdSansBoldItalic": "simsun"
  },
  "styles": [
    [
      "base",
      {
        "wordWrap": "CJK"
      }
    ],
    [
      "literal",
      {
        "wordWrap": "None"
      }
    ]
  ]
}
```

配置编译脚本

Windows用户，在make.bat中加入：

```bat
if "%1" == "pdf" (
   %SPHINXBUILD% -b pdf %ALLSPHINXOPTS% %BUILDDIR%/pdf
   if errorlevel 1 exit /b 1
   echo.
   echo.Build finished. The pdf files are in %BUILDDIR%/pdf.
   goto end
)
```

类Unix用户修改Makefile:

```shell
pdf:
    $(SPHINXBUILD) -b pdf $(ALLSPHINXOPTS) $(BUILDDIR)/pdf
    @echo
    @echo "Build finished. The PDF files are in _build/pdf."
```

输出PDF

然后一句：

make pdf
就能输出PDF了。


## 参考

[rst2pdf](http://www.hankcs.com/program/python/the-use-of-rst2pdf-to-expand-sphinx-to-generate-pdf.html)


## 错误修复

1. cannot import name '_imaging' from 'PIL'

