# http.pprof
pprof在/debug/pprof下的端点发布运行时分析数据，你可以在你的站点上访问/debug/pprof可用端点的索引。

> 这是一个调试工具，某些请求（例如收集执行跟踪）可能很慢，如果你在直播网站上使用pprof，请考虑限制访问或仅暂时启用它。

有关更多信息，请阅读Go的[pprof文档](https://golang.org/pkg/net/http/pprof/)并阅读[Profiling Go](https://blog.golang.org/profiling-go-programs)程序。

## 语法
```
pprof
```

## 例子
启用pprof端点：

```
pprof
```