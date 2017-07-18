# startup
startup executes a command when the server begins. This is useful for preparing to serve a site by running a script or starting a background process like php-fpm. (Also see shutdown.)
启动在服务器开始时执行命令。 这对于通过运行脚本或启动像php-fpm这样的后台进程来准备服务站点是非常有用的。 （另请参见关机。）

Each command that is executed at startup is blocking, unless you suffix the command with a space and &, which will cause the command to be run in the background. The output and error of the command go to stdout and stderr, respectively. There is no stdin.
在启动时执行的每个命令都是阻塞的，除非后缀有空格和＆，这将导致命令在后台运行。 命令的输出和错误分别转到stdout和stderr。 没有标准

A command will only be executed once for each time it appears in the Caddyfile.
每次只能在Caddyfile中执行一次命令。

## 语法
```
startup command
```

`command` 是一个可执行的命令，它的后面可以带有参数

## 例子
在服务开始监听之前启动php-fpm：

```
startup /etc/init.d/php-fpm start
```

在Windows系统上，当命令的路径包含空格时，你可能需要使用引号：

```
startup "\"C:\Program Files\PHP\v7.0\php-cgi.exe\" -b 127.0.0.1:9123" &
```