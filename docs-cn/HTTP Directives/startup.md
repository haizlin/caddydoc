# startup
startup 服务器开始时执行的命令，这对于通过运行脚本或启动像php-fpm这样的后台进程来服务站点是非常有用的。 （另请参见[shutdown](./shutdown.md)）

在启动时执行的每个命令都是阻塞的，除非后缀有空格和＆，这将使命令在后台运行。 命令的输出和错误分别转到stdout和stderr，没有stdin。

每次只能在Caddyfile中执行一次命令。

## 语法
```
startup command
```
`command` 一个可执行的命令，它的后面可以带有参数

## 例子
在服务开始监听之前启动php-fpm：
```
startup /etc/init.d/php-fpm start
```

在Windows系统上，当命令的路径包含空格时，你可能需要使用引号：
```
startup "\"C:\Program Files\PHP\v7.0\php-cgi.exe\" -b 127.0.0.1:9123" &
```