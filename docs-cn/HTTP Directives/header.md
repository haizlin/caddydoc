# http.header
header可以控制响应的头信息。

请注意，如果你希望从代理后端删除响应头，则必须在 [proxy](../proxy.md) 指令中执行此操作。

## 语法
```
header path name value
```

*  **path** 匹配基本的路径
*  **name** 是字段的名称，前缀用连字符(-)删除头或加号(+)来追加而不是覆盖。
*  **value** 是字段的值, 也可以使用[placeholders(占位符)](../placeholders.md)插入动态值。

这个指令可以多次使用，也可以在相同的路径分组多个自定义头字段：

```
header path {
	name value
}
```

## 例子
给所有页面的自定义的头信息：

```
header / X-Custom-Header "My value"
```

从头信息中去除“隐藏”的字段：

```
header / -Hidden
```

在指定的路径自定义多个头信息，同时删除服务器字段：

```
header /api {
	Access-Control-Allow-Origin  *
	Access-Control-Allow-Methods "GET, POST, OPTIONS"
	-Server
}
```

给所有页面添加一些安全头信息：

```
header / {
	# 启用HTTP严格传输安全性（HSTS）以强制客户端始终通过HTTPS进行连接（如果仅仅是测试就不要使用）
	Strict-Transport-Security "max-age=31536000;"
	# 启用跨站点筛选（XSS），并告诉浏览器阻止检测到攻击
	X-XSS-Protection "1; mode=block"
	# 阻止有些浏览器使用MIME-sniffing从声明的Content-Type进行类型猜测行为
	X-Content-Type-Options "nosniff"
	# 禁止网站显示在一个框架内（点击劫持保护）
	X-Frame-Options "DENY"
}
```

# 参考
[rfc2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)