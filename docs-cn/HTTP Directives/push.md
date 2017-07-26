# http.push
push 启用并配置HTTP/2服务器推送。

Server push can accelerate page loads by sending resources that the server knows the client will need, but has not yet asked for. It is not a replacement for WebSockets. It must also be configured carefully, especially with regards for client-side caching, or pushing can actually decrease performance. If a client caches a resource after the first time it is pushed, subsequent pushes of the same resource are unnecessary.

Caddy knows which resources to push either from rules you provide in the Caddyfile or from Link headers coming from some upstream.

## 语法
```
push
```

Enables server push for any request by reading the Link HTTP header of the response. Useful if you're proxying to a backend app with proxy or fastcgi, for example, that sets the Link headers for preload purposes.

To configure a basic push rule:

```
push path [resources...]
```

path is the base path of requests which to apply this rule.
resources... is a space-separated list of resources to push. If no resources are specified, then only Link headers will be used to know push assets.
To push many resources that won't fit on a single line or to change the method or headers of the synthetic request used to initiate the pushes, open a block:

```
push path [resources...] {
    method method
    header name value
    resources
}
```

path and resources are the same as above, except when specifying resources inside the block, only list one resource per line.
method specifies the method for the synthetic request that is made to initiate the push; can be GET or HEAD. Default is GET.
header adds a header to the synthetic request that is made to initiate the push; can be specified multiple times. It consists of a field name and a value. Pseudo-header fields are not allowed.

## 例子
使用Link头启用服务器推送所有请求：
```
push
```

针对所有请求推送Google Analytics脚本（不是缓存）：
```
push / /ga.js
```

将两个CSS文件推送到主页：
```
push /index.html /common.css /home.css
```

将多个资源推送到主页：
```
push /index.html {
    /resources/css/common.css
    /resources/css/home.css
    /resources/css/compiled.css
    /resources/js/main.js
    /resources/js/jquery.min.js
    /resources/images/logo.png
    /resources/images/background.jpg
}
```

指定所有推送请求的方法和头：
```
push {
    method HEAD
    header MyHeader "The value"
}
```