# http.pprof
pprof publishes runtime profiling data at endpoints under /debug/pprof. You can visit /debug/pprof on your site for an index of the available endpoints.
pprof在/ debug / pprof下的端点发布运行时分析数据。 您可以访问/ debug / pprof在您的站点上的可用端点的索引。

This is a debugging tool. Certain requests (such as collecting execution traces) can be slow. If you use pprof on a live site, consider restricting access or enabling it only temporarily.
> 这是一个调试工具。 某些请求（如收集执行跟踪）可能很慢。 如果您在现场网站上使用pprof，请考虑限制访问或仅暂时启用它。

For more information, please see Go's pprof documentation and read Profiling Go Programs.
有关更多信息，请参阅Go的pprof文档并阅读Profiling Go程序。

## 语法
```
pprof
```

## 例子
Enable pprof endpoints:
启用pprof端点：

```
pprof
```