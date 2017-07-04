# http.root
root只是指定站点的根目录，在实际中这是非常有用的，如果网站的根目录（/）与Caddy正在执行的目录不一样的时候。

默认的根路径是当前的工作目录，相对的根路径是相对于当前工作目录。

## 语法
```
root path
```

*  **path** 是设置站点根路径的目录

## 例子
服务运行的根路径是Jake的public_html目录，而不是当前的工作目录：

```
root /home/jake/public_html
```