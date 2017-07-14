# http.status
status 将状态码写入响应，它不写响应体。

## 语法
```
status code path
```

*  **code** 用于响应的HTTP状态码（必须是数字）。
*  **path** 要匹配的请求路径。

如果你有很多状态重写组合，可以通过创建表来共享状态码：

```
status code {
    path
}
```

每行书写一个基础路径，这个基础路径应该写入状态码。

## 例子
对于全部的状态：

```
status 404 /
```

隐藏一个秘密文件夹：

```
status 404 /secrets
```

隐藏多个文件夹：

```
status 404 {
    /hidden
    /secrets
}
```