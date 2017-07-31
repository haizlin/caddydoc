# http.markdown 
markdown 将[Markdown](http://daringfireball.net/projects/markdown/)文件作输出为HTML页面，你可以指定整个自定义模板，或者也可以指定要在页面上使用的CSS和JavaScript文件，以给他们一个自定义的外观和行为。

## 语法
```
markdown [basepath] {
	ext      extensions...
	[css|js] file
	template [name] path
	templatedir defaultpath
}
```

*  **basepath** 是基本路径匹配，如果请求的URL不以这个路径前缀，则Markdown不会激活，默认是站点根目录。
*  **extensions...** 是以空格分隔的文件扩展名列表，用作Markdown（默认是.md，.markdown和.mdown）。这和使用默认文件扩展名的ext指令不同。
*  **css** 表示下一个参数是要在页面上使用的CSS文件。
*  **js** 表示下一个参数是要包含在页面中的JavaScript文件。
*  **file** 添加到页面中的JS或CSS文件。
*  **template** 定义了指定名称的模板位于指定的路径，如果要指定默认模板，请忽略名称，Markdown文件可以使用Front Matter的名称来选择一个模板。
*  **templatedir** 将默认路径设置为在列出模板时使用的给定默认路径。

你可以多次使用js和css参数将更多文件添加到默认模板，如果你想接受默认值，你完全可以省略大括号。

## Front Matter（文档元数据）
Markdown文件可以从Front Matter开始，这是一个特殊格式的文件元数据块。 例如，它可以描述用于呈现文件的HTML模板，或者定义标题标签的内容，Front Matter可以是YAML，TOML或JSON格式。

TOML Front Matter 开始和结束使用+++：
```
+++
template = "blog"
title = "Blog Homepage"
sitename = "A Caddy site"
+++
```

YAML格式被---包围：
```
---
template: blog
title: Blog Homepage
sitename: A Caddy site
---
```

JSON只是{和}：
```
{
	"template": "blog",
	"title": "Blog Homepage",
	"sitename": "A Caddy site"
}
```

Front Matter字段 "author", "copyright", "description"和"subject"将使用在渲染页面的`<meta>`标签上。

## Markdown模板
模板文件只是具有模板标签的HTML文件，称为操作，可以根据正在提供的文件插入动态内容。 可以从`{{.Doc.variable}}`之类的模板访问元数据中定义的变量，其中"variable"是变量的名称，这个变量`.Doc.body`保存markdown文件的正文。

这是一个简单的示例模板（随意写的）：
```
<!DOCTYPE html>
<html>
	<head>
		<title>{{.Doc.title}}</title>
	</head>
	<body>
		Welcome to {{.Doc.sitename}}!
		<br><br>
		{{.Doc.body}}
	</body>
</html>
```
除了这些模板操作，你还可以在Markdown模板中使用所有标准的Caddy模板操作，确保清理HTML中呈现的任何内容（使用html，js和urlquery函数）！

## 例子
在没有特殊格式的情况下，在/blog中启用Markdown页面（假设.md是Markdown扩展名）：

```
markdown /blog
```

与上述相同，但使用了自定义的CSS和JS文件：
```
markdown /blog {
	ext .md .txt
	css /css/blog.css
	js  /js/blog.js
}
```

使用自定义模板：
```
markdown /blog {
	template default.html
	template blog  blog.html
	template about about.html
}
```