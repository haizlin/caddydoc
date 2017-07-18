# http.redir
redir sends the client an HTTP redirect status code if the URL matches the specified pattern. It is also possible to make a redirect conditional.
如果URL匹配指定的模式，redir会向客户端发送HTTP重定向状态代码，也可以使重定向有条件。

## 语法
```
redir from to [code]
```

from is the request path to match (it must match exactly, except for /, which is a catch-all).
to is the path to redirect to (may use request placeholders).
code is the HTTP status code to respond with; must be in the range [300-308] excluding 306. May also be meta to issue meta tag redirect for browsers. The default status code is 301 Moved Permanently.
To create a permanent, "catch-all" redirect, omit the from value:

```
redir to
```

If you have a lot of redirects, share a redirect code by making a table:

```
redir [code] {
	from to [code]
}
```

Each line defines a redirect and may optionally overwrite the redirect code defined at the top of the table. If no redirect code is specified, the default is used.

一组重定向也可以有条件的：
```
redir [code] {
	if    a cond b
	if_op [and|or]
	...
}
```

if specifies a rewrite condition. Multiple ifs are AND-ed together by default. a and b are any string and may use request placeholders. cond is the condition, with possible values explained in rewrite (which also has an if statement).
if_op specifies how the ifs are evaluated; the default is and.
Preserving Path
By default, redirects are from precisely matching paths to the precise location you've defined. You can preserve the path or other portions of the request URL by using replaceable values, such as {uri} or {path}, in any "to" argument. Only request placeholders are supported.

## 例子
When a request comes in for /resources/images/photo.jpg, redirect to /resources/images/drawing.jpg with HTTP 307 (Temporary Redirect) status code:

```
redir /resources/images/photo.jpg /resources/images/drawing.jpg 307
```

Redirect all requests to https://newsite.com while preserving the request URI:

```
redir https://newsite.com{uri}
```

Defining multiple redirections that share a 307 status code, except the last one:

```
redir 307 {
	/foo     /info/foo
	/todo    /notes
	/api-dev /api       meta
}
```

Redirect only if the forwarded protocol is HTTP:

```
redir 301 {
	if {>X-Forwarded-Proto} is http
	/  https://{host}{uri}
}
```