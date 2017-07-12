# http.markdown
markdown serves Markdown files as HTML pages on demand. You can specify whole custom templates or just the CSS and JavaScript files to be used on the pages to give them a custom look and behavior.
markdown将[Markdown](http://daringfireball.net/projects/markdown/)文件作为HTML页面提供。 您可以指定整个自定义模板，也可以指定要在页面上使用的CSS和JavaScript文件，以给他们一个自定义的外观和行为。

## 语法
```
markdown [basepath] {
	ext      extensions...
	[css|js] file
	template [name] path
}
```

basepath is the base path to match. Markdown will not activate if the request URL is not prefixed with this path. Default is site root.
extensions... is a space-delimited list of file extensions to treat as Markdown (defaults to .md, .markdown, and .mdown); this is different from the ext directive which assumes a missing file extension.
css indicates that the next argument is a CSS file to use on the page.
js indicates that the next argument is a JavaScript file to include on the page.
file is the JS or CSS file to add to the page.
template defines a template with the given name to be at the given path. To specify the default template, omit name. Markdown files can choose a template by using the name in its front matter.
You can use the js and css arguments more than once to add more files to the default template. If you want to accept defaults, you should completely omit the curly braces.

Front Matter (Document Metadata)
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

Here is a simple example template (contrived):

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

With custom templates:
使用自定义模板：

```
markdown /blog {
	template default.html
	template blog  blog.html
	template about about.html
}
```