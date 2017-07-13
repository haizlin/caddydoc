# http.expvar
expvar以JSON格式公开关于运行时或当前进程状态的变量，默认端点是/debug/vars。 默认情况下，它会报告内存统计信息，用于运行Caddy的命令和goroutine的数量。

> 这是一个调试工具, 尽管在实时网站上使用它通常是安全的，但请注意不要透露任何敏感信息，另请注意，插件可能会添加到已发布的值列表中。

更多相关的信息，请参阅Go的[expvar文档](https://golang.org/pkg/expvar/)

## 语法
```
expvar [path]
```
*  **path** 是服务变量的端点，默认是/debug/vars。

## 例子
在默认路径下启用expvar：

```
expvar
```

在自定义路径中启用expvar：

```
expvar /stats
```