# http.rewrite
rewrite performs internal URL rewriting. This allows the client to request one resource but actually be served another without an HTTP redirect. Rewrites are invisible to the client.
重写执行内部URL重写，这允许客户端请求一个资源，但实际上是另一个资源，而不需要HTTP重定向，重写是客户不可见的。

There are simple rewrites (fast) and complex rewrites (slower), but they're powerful enough to accommodate most dynamic back-end applications.
有简单的重写（快速）和复杂的重写（较慢），但它们足够强大以适应大多数动态后端应用程序。

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

basepath is the base path to match before rewriting with regular expression. Default is /.
regexp (shorthand: r) will match the path with the given regular expression pattern. Extremely high-load servers should avoid using regular expressions.
extensions... is a space-separated list of file extensions (prepended with a dot) to include or ignore. Prefix an extension with ! to exclude an extension. The forward slash  / symbol matches paths without file extensions.
if specifies a rewrite condition. Multiple ifs are AND-ed together. a and b are any string and may use request placeholders. cond is the condition, with possible values explained below.
if_op specifies how the ifs are evaluated; the default is and.
destinations... is one or more space-separated paths to rewrite to, with support for request placeholders as well as numbered regular expression captures such as {1}, {2}, etc. Rewrite will check each destination in order and rewrite to the first destination that exists. Each one is checked as a file or, if ends with /, as a directory. The last destination will act as default if no other destination exists.

## "if" 条件
The if keyword is a powerful way to describe your rule. It takes the format a cond b, where the values a and b are separated by cond, a condition. The condition can be any of these:

* `is` = a 等于 b
* `not` = a 不等于 b
* `has` = a has b as a substring (b is a substring of a)
* `not_has` = b is NOT a substring of a
* `starts_with` = b is a prefix of a
* `not_starts_with` = b is NOT a prefix of a
* `ends_with` = b is a suffix of a
* `match` = a matches b, where b is a regular expression
* `not_match` = a does NOT match b, where b is a regular expression

# 例子
将所有内容重写为/index.php。 （rewrite / /index.php{uri} 只匹配/）

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