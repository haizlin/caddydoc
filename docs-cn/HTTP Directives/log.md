# http.log
log启用请求日志记录，请求日志在一些术语中也被称为访问日志。

## 语法
不需要任何参数，所有请求的访问日志都以通用的格式写到access.log中：

```
log
```

自定义日记文件的位置：
```
log file
```
*  **file** 是相对于当前工作目录创建（或附加到）日志文件的路径， 有关如何指定输出位置的更多详细信息，请参阅[日记的位置](#destination)， 默认的是access.log。

将日志限制在某些请求或更改日志格式：

```
log path file [format]
```

*  **path** 为了记录而匹配的基本请求路径。
*  **file** 相对于当前工作目录创建（或附加到）的日志文件。
*  **format** 要使用的日志格式，默认为通用日志格式。

大型日志文件自动轮转，你可以通过打开一个语句块来自定义日志轮转：

```
log path file [format] {
	rotate_size mb
	rotate_age  days
	rotate_keep count
}
```

* rotate_size 轮转之前日志文件必须达到的大小（以兆字节为单位）。
*  **rotate_age** 保留轮转日志文件的天数。
*  **rotate_keep** 要保留的最大轮转日志文件数，旧的轮转日志文件被切割。

## 日记格式
你可以使用任何[占位符](../HTTP Server/placeholder.md)的值指定自定义日志格式, Log支持请求和响应的占位符。

目前有两种预定的格式。

*  **{common}** (默认)  

```
{remote} - {user} [{when}] "{method} {uri} {proto}" {status} {size}
```

*  **{combined}** - {common} 附加  

```
"{>Referer}" "{>User-Agent}"
```

## 日记的位置 <span id="destination"></span>
日记的位置可以是以下几点：

* 相对于当前工作目录的文件名
* `stdout` 或 `stderr` 写入控制台
* `visible` 将错误写入（包括完整堆栈跟踪（如果适用））响应中（除了本地调试之外，不推荐）
* `syslog` 写入本地系统日志（除了Windows）
* `syslog://host[:port]` 通过UDP写入本地或远程syslog服务器
* `syslog+udp://host[:port]` 和上面的相同
* `syslog+tcp://host[:port]` 通过TCP写入本地或远程syslog服务器

默认日记位置是`stderr(标准错误输出)`。

## 日志轮转
日志有可能会把磁盘填满，为了减少这种情况，请求日志根据这个默认的配置自动轮转（"rolled"）：

```
rotate_size 100 # Rotate a log when it reaches 100 MB
rotate_age  14  # Keep rotated log files for 14 days
rotate_keep 10  # Keep at most 10 rotated log files
```
您可以指定这些子目录来自定义日志轮转。

## 例子

将所有的请求都记录到access.log：
```
log
```

将所有的请求都记录到stdout：
```
log stdout
```

自定义日记格式：
```
log / stdout "{proto} Request: {method} {path}"
```

预定格式：
```
log / stdout "{combined}"
```

关于轮转：
```
log requests.log {
	rotate_size 50 # Rotate after 50 MB
	rotate_age  90 # Keep rotated files for 90 days
	rotate_keep 20 # Keep at most 20 log files
}
```