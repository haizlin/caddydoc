# http.browse
browse可以在指定的路径中开启目录浏览, 它显示的是在没有索引文件的目录的文件列表，在其他服务器软件中，这通常称为索引。

默认情况下，文件列表是被禁用的，当索引文件不存在的时候，如果直接请求访问目录将返回404错误。

如果用户更改它们时，这个中间件可能会设置Cookie以保留UI首选项。

## 语法
```
browse [path [tplfile]]
```

*  **path** 匹配的是基本路径，在这个基本路径中的任何目录都可以浏览。  
*  **tplfile** 使用模板文件的位置。  

如果没有指定模板文件时，将使用默认的模板。 没有任何参数的情况下，在整个站点上启用浏览（path=/）。

## 模板格式
模板只是一个包含标签的HTML文件，这些标签可以被解析并执行以显示动态的内容。 这个指令支持Caddy的模板标签以及特定于browse指令的一些其他标签，你可以使用模板标签去渲染这个结构类型（请注意，有些辅助的方法是可以用的）

这是一个非常简单的模板示例：

```
<html>
	<head>
		<title>{{html .Name}}</title>
	</head>
	<body>
		{{if .CanGoUp}}<a href="..">Up one level</a><br>{{end}}
		<h1>{{.Path}}</h1>
		{{range .Items}}
		<a href="{{html .URL}}">{{html .Name}}</a><br>
		{{end}}
	</body>
</html> 
```

...但默认模板会更好。

请注意，名称和URL已经被过滤，以便在浏览器中进行安全渲染，假设模板是可信任的，所以如果你的文件名不受信任，请确保在使用HTML文档前它们已经被转义。

## JSON响应
你可以通过在你的请求头中使用application/json来请求JSON的形式代替浏览页面：

```
$ curl -H "Accept: application/json" 'localhost:2015/?limit=1'
[{"IsDir":true,"Name":".git","Size":476,"URL":".git/","ModTime":"2015-09-11T03:20:09+03:00","Mode":2147484141}]
```

上面的例子演示了如何使用JSON，以及如何通过一个名为limit的查询来限制我们想要的条目数。 为了显示整个列表，请忽略查询条件的限制。

## 例子
允许在没有索引文件的所有文件夹中浏览目录列表：

```
browse
```

在/photos目录中，通过一个自定义模板显示相册的内容：

```
browse /photos ../photo_album.tpl
```