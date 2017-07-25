# http.internal
internal 保护指定目录中的所有资源不被外部的请求访问到，如果浏览器（或者任何客户端）直接请求受保护目录中的资源将收会到404 Not Found 的状态提示。

因为这个指令支持X-Accel-Redirect头，所以它通常与后端代理一起使用，请求内部不同的URL时可能会被重定向到可以设置X-Accel-Redirect header头的代理，当Caddy知道这是从代理来的时候，它将允许访问内部资源并将它发送给客户端，这有时也被称为X-Sendfile。

此模式处理请求允许后端代理执行日志记录，身份验证和其他事情，而客户端不必处理重定向。

## 语法
```
internal path
```
*  **path** 保护外部请求的基本路径


## 例子
保护/internal目录下的所有内容不能被直接访问：

```
internal /internal
```

Caddyfile保护的部分资源，但允许代理授权访问它们（服务侦听9000端口必须设置X-Accel-Redirect）：
```
internal /internal
proxy    /redirect http://localhost:9000
```