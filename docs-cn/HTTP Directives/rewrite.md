# http.rewrite
rewrite 执行内部URL重写，允许客户端请求一个资源，但实际上是另一个资源，而不需要HTTP重定向，重写对于客户端是不可见的。

有简单的重写（快速）和复杂的重写（较慢），但它们足够强大以适应大多数的动态应用程序。

## 语法
```
rewrite from to
```
*  **from** 是要匹配的真实路径
*  **to** 要重写的目标路径（用于响应的资源）

高级用户可以使用一个语句块进行复杂的重写规则：

```
rewrite [basepath] {
	regexp pattern
	ext    extensions...
	if     a cond b
	if_op  [and|or]
	to     destinations...
}
```

*  **basepath** 是用正则表达式重写之前匹配的基础路径，默认是/。
*  **regexp** （简写是：r）将匹配给定正则表达式模式的路径，超高负载的服务器应避免使用正则表达式。
extensions... is a space-separated list of file extensions (prepended with a dot) to include or ignore. Prefix an extension with ! to exclude an extension. The forward slash  / symbol matches paths without file extensions.
*  **extensions...** 是包含或忽略的文件扩展名（带点）的空格分隔列表。 前缀是!时则排除扩展名，正斜杠/符号匹配没有文件扩展名的路径。
*  **if** 指定了重写的条件，多个ifs和AND结合在一起，a和b是任意字符串，可以使用[请求占位符](../HTTP Server/placeholders)。 cond是条件，可能的值如下。
*  **if_op** 指定ifs如何求值，默认是and。
destinations... is one or more space-separated paths to rewrite to, with support for request placeholders as well as numbered regular expression captures such as {1}, {2}, etc. Rewrite will check each destination in order and rewrite to the first destination that exists. Each one is checked as a file or, if ends with /, as a directory. The last destination will act as default if no other destination exists.

*  **destinations...** 是一个或多个以空格分隔的路径来重写，支持[请求占位符](../HTTP Server/placeholders)以及编号的正则表达式捕获，如{1}，{2}等。重写将按顺序检查每个目标位置并重写 存在的第一个目的地。 每个作为文件检查，或者以/作为目录结尾。 如果没有其他目的地，最后一个目的地将作为默认值。

## "if" 条件
if关键字是书写规则的强大方式，它的格式是a cond b，其中值a和b由cond条件分隔，条件可以是以下任何一种：

* `is` = a 等于 b
* `not` = a 不等于 b
* `has` = a有b作为子字符串（b是a的子字符串）
* `not_has` = b不是a的子字符串
* `starts_with` = b是a的前缀
* `not_starts_with` = b不是a的前缀
* `ends_with` = b是a的后缀
* `match` = a匹配b，其中b是个正则表达式
* `not_match` = a不匹配b，其中b是个正则表达式

# 例子
将所有内容重写为/index.php，（rewrite / /index.php{uri} 只匹配/）

```
rewrite / {
	regexp .*
	to /index.php{uri}
}
```

当请求的链接是/mobile时，实际上访问到的内容是/mobile/index的。

```
rewrite /mobile /mobile/index
```

If the file is not favicon.ico and it is not a valid file or directory, serve the maintenance page if present, or finally, rewrite to index.php.  
如果文件不是favicon.ico，并且它不是有效的文件或目录，请提供维护页面（如果存在），或者最后重写为index.php。

```
rewrite {
	if {file} not favicon.ico
	to {path} {path}/ /maintenance.html /index.php
}
```

If user agent includes "mobile" and path is not a valid file/directory, rewrite to the mobile index page.  
如果用户代理包含了"mobile"字符串，并且路径不是有效的文件/目录，请重写到移动索引页。

```
rewrite {
	if {>User-agent} has mobile
	to {path} {path}/ /mobile/index.php
}
```

Rewrite /app to /index with a query string. {1} is the matched group (.*).
将/app下的请求重写到/index的查询字符串中，{1}是匹配组(.*)。

```
rewrite /app {
	r  (.*)
	to /index?path={1}
}
```

将/app/example的请求重写到/index.php?category=example。

```
rewrite /app {
	r  ^/(\w+)/?$
	to /index?category={1}
}
```