# http.errors
errors 允许你设置自定义错误页面并且开启错误日记。

如果没有这个中间件，错误的响应（HTTP status >= 400）将不会被记录，并且客户端将会收到明文的错误信息。

使用错误日志，将记录每个错误的信息，方便定位出现哪些错误细节，而不会将这些详细信息暴露给客户端。错误页面，你可以显示自定义错误消息，并指示访问者做什么。当指定自定义错误页面时，会自动启用错误日志记录。

## 语法
```
errors [logfile]
```

logfile 错误日志文件的路径，相对于当前工作目录生成（或追加）的，有关如何指定输出位置的更多详细信息，请参阅[日志位置](#destination)，默认是stderr。

要指定自定义错误页面，请使用语句块：  
```
errors [logfile] {
	code file
	rotate_size mb
	rotate_age  days
	rotate_keep count
}
```

*  **code** 可以是HTTP状态代码（默认错误页面为4xx，5xx或*）。
*  **file** 是错误页面的静态HTML文件（路径是相对于站点根目录）。
*  **rotate_size** 轮转之前日志文件必须达到的大小（以兆字节为单位）。
*  **rotate_age** 保留轮转日志文件的天数。
*  **rotate_keep** 要保留的最大轮转日志文件数，旧的轮转日志文件被切割。

## 日记位置 <span id="destination"></span>
日记的位置可以是以下几种：

* 相对于当前工作目录的文件名
* `stdout` 或 `stderr` 写入控制台
* `visible` 将错误写入（包括完整堆栈跟踪（如果适用））响应中（除了本地调试之外，不推荐）
* `syslog` 写入本地系统日志（除了Windows）
* `syslog://host[:port]` 通过UDP写入本地或远程syslog服务器
* `syslog+udp://host[:port]` 和上面的相同
* `syslog+tcp://host[:port]` 通过TCP写入本地或远程syslog服务器

## 日志轮转
日志有可能会把磁盘填满，为了减少这种情况，请求日志根据这个默认的配置自动轮转（"rolled"）：

```
rotate_size 100 # 当日志达到100 MB时轮转日志
rotate_age  14  # 保持轮转日志文件14天
rotate_keep 10  # 最多保留10个轮转的日志文件
```

您可以指定这些子目录来自定义日志轮转。

## 例子
将错误记录到error.log中：
```
errors
```

将错误记录到父目录中的自定义文件中：
```
errors ../error.log
```

记录错误并提供自定义错误页面：
```
errors {
	404 404.html # 没有找到
	500 500.html # 服务器错误
}
```

记录错误到自定义的错误日记文件并提供自定义错误页面:
```
errors ../error.log {
	404 404.html # 没有找到
	500 500.html # 内部服务器错误
}
```

定义一个默认、捕获所有错误的页面：
```
errors {
	* default_error.html
}
```

使客户端可以看到错误（仅用于调试）：

```
errors visible
```

自定义错误日记的轮转：

```
errors {
	rotate_size 50 # 50 MB后轮转
	rotate_age  90 # 保持轮转文件90天
	rotate_keep 20 # 最多保留20个轮转的日志文件
}
```