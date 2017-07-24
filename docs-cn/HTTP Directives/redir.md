# http.redir
如果URL匹配指定的模式，redir会向客户端发送HTTP重定向状态码，也可以使重定向有条件。

## 语法
```
redir from to [code]
```
*  **from** 要匹配的请求路径（它必须完全匹配，除了/，这是一个全部）。
*  **to** 要重定向到的路径（可以使用请求占位符）。
*  **code** 用于响应HTTP的状态码，必须在[300-308]的之间（不包括306）。也可以是针对浏览器的meta标记重定向，默认状态码为301表示永久移动。

要创建永久的"catch-all"重定向，请忽略from值：
```
redir to
```

如果你有很多重定向，可以通过制表来共享重定向码：
```
redir [code] {
	from to [code]
}
```

每行都可以定义一个重定向，并可以可选地覆盖在表顶部定义的重定向码，如果未指定重定向代码，则使用默认值。

一组重定向也可以有条件的：
```
redir [code] {
	if    a cond b
	if_op [and|or]
	...
}
```

*  **if** 指定重写的条件，默认情况下，多个ifs都是和的关系。 a和b是任意字符串，可以使用请求占位符，cond是条件，可能的值在重写中说明（也有一个if语句）。
*  **if_op** 指定如何确定ifs的关系，默认是和。

## 保护路径
默认情况下，重定向是从精确匹配的路径到你定义的精确位置，你可以在任意"to"的参数中使用可替换值（例如{uri}或{path}）来保留请求URL的路径或其他部分，仅支持请求占位符。

## 例子
当请求/resources/images/photo.jpg时，使用HTTP 307状态码（临时重定向）重定向到/resources/images/drawing.jpg：
```
redir /resources/images/photo.jpg /resources/images/drawing.jpg 307
```

将所有请求重定向到https://newsite.com，同时保留请求的URI：
```
redir https://newsite.com{uri}
```

定义多个重定向的307状态码，除了最后一个：
```
redir 307 {
	/foo     /info/foo
	/todo    /notes
	/api-dev /api       meta
}
```

只有转发协议是HTTP时才重定向：
```
redir 301 {
	if {>X-Forwarded-Proto} is http
	/  https://{host}{uri}
}
```