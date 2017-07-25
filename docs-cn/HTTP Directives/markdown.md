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
template defines a template with the given name to be at the given path. To specify the default template, omit name. Markdown files can choose a template by using the name in its front matter.
*  **template** 定义具有给定名称的模板位于给定路径，如果要指定默认模板，请忽略名称。 Markdown文件可以通过使用其前面的名称来选择一个模板。

templatedir sets the default path with the given defaultpath to be used when listing templates.
*  **templatedir** 将默认路径设置为在列出模板时使用的给定默认路径。

You can use the js and css arguments more than once to add more files to the default template. If you want to accept defaults, you should completely omit the curly braces.
你可以多次使用js和css参数将更多文件添加到默认模板，如果你想接受默认值，你应该完全省略大括号。

Front Matter (Document Metadata)
## 前页（文件元数据）
Markdown files may begin with front matter, which is a specially-formatted block of metadata about the file. For example, it could describe the HTML template to use to render the file, or define the contents of the title tag. Front matter can be in YAML, TOML, or JSON formats.

TOML front matter starts and ends with +++:

```
+++
template = "blog"
title = "Blog Homepage"
sitename = "A Caddy site"
+++
```
YAML is surrounded by ---:

```
---
template: blog
title: Blog Homepage
sitename: A Caddy site
---
```

JSON is simply { and }:

```
{
	"template": "blog",
	"title": "Blog Homepage",
	"sitename": "A Caddy site"
}
```
The front matter fields "author", "copyright", "description", and "subject" will be used to create <meta> tags on the rendered pages.

Markdown Templates
Template files are just HTML files with template tags, called actions, that can insert dynamic content depending on the file being served. The variables defined in metadata can be accessed from the templates like {{.Doc.variable}} where 'variable' is the name of the variable. The variable .Doc.body holds the body of the markdown file.

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
Along with these template actions, all the standard Caddy template actions are available to you in Markdown templates. Be sure to sanitize anything you render as HTML (use the html, js, and urlquery functions)!


## 例子
Serve Markdown pages in /blog with no special formatting (assumes .md is the Markdown extension):

```
markdown /blog
```

Same as above, but with custom CSS and JS files:

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