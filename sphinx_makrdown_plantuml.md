
# 在sphinx下的markdown中使用plantuml

## 使用特殊关键词{uml}

在sphinx中使用markdown中的plantuml时，其格式如下：

    ```{uml}
    a->b
    b->c
    ```

其样式如下：

```{uml} 
a->b
b->c
```

需要注意的是，\``` 后面跟随的关键字不能为`plantuml`，需要为sphinx能识别的`{uml}`，否则无法生成图形


    这里需要特别说明的是，markdown文件不会直接转换成html，而是先转换成rst格式，再转换成html

## 添加 plantuml 关键字支持

当在sphinx中使用myst_parser来解析markdown时，若想统一使用markdown中的plantuml扩展，即markdown中的关键词Plantuml不想修改时，则可以改动myst_parser中的"docutils_renderer.py"文件中`render_directive`函数实现来支持`plantuml`关键词。

修改的文件为`python{version}/site-packages/myst_parser/docutils_renderer.py`。

在`render_directive`函数中的`firsh_line`下面添加如下代码：
```python
	if (first_line[0] == "plantuml" or first_line[0] == "{plantuml}" or first_line[0] == "uml"):
  		name = "uml"
  	else:
   		name = first_line[0][1:-1]
```

同时需要在`render_fence`函数中，将plantuml关键字段的过滤加上，否则不会执行到`render_directive`函数中去。

其修改的地方如下：
```python
     elif (
		not self.config.get("commonmark_only", False)
		and language.startswith("{")
		and language.endswith("}")
		or language == "plantuml"
	):
```

修改后的效果如下：

    ```plantuml
    a1->b1
    b1->c1
    ```

其样式如下：

```plantuml 
a1->b1
b1->c1
```

其详细patch如下：
```txt
diff -Naur a/docutils_renderer.py b/docutils_renderer.py
--- a/docutils_renderer.py	2021-12-12 02:34:00.392720842 +0800
+++ b/docutils_renderer.py	2021-12-12 02:29:59.366903990 +0800
@@ -459,6 +459,7 @@
             not self.config.get("commonmark_only", False)
             and language.startswith("{")
             and language.endswith("}")
+            or language == "plantuml"
         ):
             return self.render_directive(token)
 
@@ -984,7 +985,11 @@
     def render_directive(self, token: SyntaxTreeNode) -> None:
         """Render special fenced code blocks as directives."""
         first_line = token.info.split(maxsplit=1)
-        name = first_line[0][1:-1]
+        
+        if (first_line[0] == "plantuml" or first_line[0] == "{plantuml}" or first_line[0] == "uml"):
+            name = "uml"
+        else:
+            name = first_line[0][1:-1]
         arguments = "" if len(first_line) == 1 else first_line[1]
         content = token.content
         position = token_line(token)

```


命令行下mardown文件手动生成html

```shell
markdown_py -x plantuml_markdown reference.md -f test1.html
```
