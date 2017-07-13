# http.errors
errors 允许你设置自定义错误页面并且开启错误日记。

Without this middleware, error responses (HTTP status >= 400) are not logged and the client receives a plaintext error message.
如果没有这个中间件，错误的响应（HTTP status> = 400）将不会被记录，并且客户端将会收到明文的错误消息。

Using an error log, the text of each error will be recorded so you can determine what is going wrong without exposing those details to the clients. With error pages, you can present custom error messages and instruct your visitor what to do. When you specify custom error pages, error logging is automatically enabled.
使用错误日志，将记录每个错误的文本，以便您可以确定出现哪些错误，而不会将这些详细信息暴露给客户端。 使用错误页面，您可以显示自定义错误消息，并指示访问者做什么。 指定自定义错误页面时，会自动启用错误日志记录。

## 语法
```
errors [logfile]
```

logfile is the path to the error log file to create (or append to), relative to the current working directory. See Log Destination for more details about how to specify an output location. Default is stderr.
logfile是相对于当前工作目录创建（或附加到）的错误日志文件的路径。 有关如何指定输出位置的更多详细信息，请参阅日志目标。 默认是stderr。

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
file is the static HTML file of the error page (path is relative to site root).
rotate_size is the size in megabytes a log file must reach before rolling it.
rotate_age is how long in days to keep rotated log files.
rotate_keep is the maximum number of rotated log files to keep; older rotated log files get pruned.
Log Destination
The log destination can be one of a few things:

a filename relative to the current working directory
stdout or stderr to write to the console
visible to write the error (including full stack trace, if applicable) to the response (NOT recommended except for local debugging)
syslog to write to the local system log (except on Windows)
syslog://host[:port] to write via UDP to a local or remote syslog server
syslog+udp://host[:port] is the same as above
syslog+tcp://host[:port] to write via TCP to local or remote syslog server
The default log destination is stderr.

## 日记回滚
Logs have the potential to fill the disk. To mitigate this, error logs are rotated ("rolled") automatically according to this default configuration:

```
rotate_size 100 # Rotate a log when it reaches 100 MB
rotate_age  14  # Keep rotated log files for 14 days
rotate_keep 10  # Keep at most 10 rotated log files
```

You can specify these subdirectives to customize log rolling.
您可以指定这些子目录来自定义日志滚动。

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
	404 404.html # Not Found
	500 500.html # 服务器错误
}
```

记录错误到自定义的错误日记文件并提供自定义错误页面:
```
errors ../error.log {
	404 404.html # Not Found
	500 500.html # Internal Server Error
}
```

定义一个默认、捕获所有错误的页面：
```
errors {
	* default_error.html
}
```

Make errors visible to the client (for debugging only):

```
errors visible
```

Customize error log rolling:

```
errors {
	rotate_size 50 # Rotate after 50 MB
	rotate_age  90 # Keep rotated files for 90 days
	rotate_keep 20 # Keep at most 20 log files
}
```