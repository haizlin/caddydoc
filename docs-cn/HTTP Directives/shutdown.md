# shutdown
shutdown 是在服务器退出时执行的命令，这对于执行清理或停止像php-fpm这样的后台进程很有用。 （另见[startup](./startup.md)）

在关机时执行的每个命令都是阻塞的，命令的输出和错误分别转到stdout和stderr，没有stdin

请注意，如果Caddy被强制终止，例如使用操作系统提供的"强制退出"功能，则shutdown命令将不会执行。 但是，典型的SIGINT（Ctrl+C）将允许执行shutdown命令。

尽管这个指令可以多个主机共享，该命令只会在Caddyfile中每次出现时执行一次。

## 语法
```
shutdown command
```
*  **command** 执行的命令，后面可能带有参数

## 例子
当服务器退出时停止php-fpm：
```
shutdown /etc/init.d/php-fpm stop
```