# http.mime
mime是根据请求中的文件扩展名来设置响应的Content-Type。

正常情况下，通过嗅探内容，静态文件会自动检测到Content-Type，但并不总是可能的。 如果你遇到错误的Content-Type或正在提供静态文件以外的内容响应，那么可以使用这个中间件设置正确的Content-Type。

## 语法
```
mime ext type
```

*  **ext** 匹配文件的扩展，包括前缀的'.'
*  **type**  Content-Type的值

如果你要设置很多MIME类型，可以使用语句块：

```
mime {
	ext type
}
```

每行定义一个MIME扩展类型对，你可以在mime块有多行。

## 例子
自定义Flash文件的Content-Type：

```
mime .swf application/x-shockwave-flash
```

对于多个文件：

```
mime {
	.swf application/x-shockwave-flash
	.pdf application/pdf
}
```