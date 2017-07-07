# Placeholders 
有些指令允许你在Caddyfile中使用占位符，以便为每个请求填写不同的值。 例如，这个值`{path}`将被请求URL的路径部分替换，这些也称为可替换值。

> 这些占位符只适用于支持它们的指令，检查你的指令的文档，看看是否支持占位符。

## 请求占位符
这些值是从请求获得的。


| 点位符   		| 值         |
| ------------- |:-------------|
| {~cookie}		| cookie的值，其中 "cookie"是cookie名称 |
| {dir}			| 请求文件的目录 (来自URI的请求)      |
| {file}		| 请求文件的名称 (来自URI的请求)      |
| {fragment}	| 以"#"开始的URL的最后一部分    |
| {>Header}		| 任何请求头，其中"Header"是头字段名称      |
| {host}		| 请求的主机值      |
| {hostname}	| 正在处理请求的主机的名称      |
| {hostonly}	| 与{host}相同，但没有端口信息      |
| {method}		| 请求的方法 (GET, POST等)      |
| {mitm}		| HTTPS是否拦截，可能，不太可能或未知      |
| {path_escaped}| {path}的查询转义变体      |
| {port}		| 客户端的端口号      |
| {proto}		| 协议串（例如"HTTP/1.1"）      |
| {query}		| URL的部分查询字符串，不带前缀"?"     |
| {query_escaped}| The query-escaped variant of {query}      |
| {?key}		| 来自查询字符串的"key"参数的值     |
| {remote}		| 客户端的IP地址      |
| {request}		| 整个HTTP请求（没有body），压缩成一行      |
| {request_body}| 请求正文，压缩成一行（最大长度为100 KB;仅限JSON或XML）      |
| {rewrite_path}| 与{path}相同，但是重写后路径的值      |
| {rewrite_path_escaped}		| {rewrite_path}的查询转义变体      |
| {rewrite_uri}	| 请求的URI发生任何重写后（包括路径，查询字符串和片段）    |
| {rewrite_uri_escaped}| {rewrite_uri}的查询转义变体      |
| {scheme}		| 所使用的protocol/scheme（通常为http或https）      |
| {uri}			| 请求的URI（包括路径，查询字符串和片段）      |
| {uri_escaped}	| {uri}的查询转义变体      |
| {when}		| 在本地的时间戳格式为02/Jan/2006:15:04:05 -0700      |
| {when_iso}	| 时间戳格式为2006-01-02T15:04:05Z in UTC     |


## 响应占位符
这些值是从响应中获得的，只能在一些指令中使用，所以在使用它们之前，请确保你的指令支持响应占位符。

| 占位符		|		值		|
| ------------- |:-------------|
| {latency}		| 服务器处理请求大致时间的可读格式     |
| {latency_ms}		| 服务器用于处理请求的大概时间（以毫秒为单位）     |
| {size}		| 响应体的大小      |
| {status}		| HTTP的响应状态码      |

