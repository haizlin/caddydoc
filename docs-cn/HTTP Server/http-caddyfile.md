# The HTTP Caddyfile
这个文档介绍了HTTP服务器如何使用Caddyfile，如果还没有使用过Caddyfile，请先看下[Caddyfile](../Using Caddy/tutorial/caddyfile)的教程或者阅读下[Caddy](/docs/caddyfile)的语法。

**主题**  
1. [站点地址](#addresses)  
2. [路径匹配](#path-matching)  
3. [指令](#directives)  
4. [占位符](#placeholders)  

## 站点地址 <span id="addresses"></span>
HTTP服务器使用站点地址作为标签，地址以scheme://host:port/path的形式，只有一个是可选的。

主机部分通常是localhost或域名，默认端口为2015（除非站点符合自动HTTPS的条件，在这种情况下，它将更改为443）。 这个scheme部分是指定端口的另一种方式，有效的scheme协议是分别表示端口80和443的"http"或"https"，如果指定了scheme协议和端口，则端口优先。例如（这个表假设自动的HTTPS在其符合条件的情况下应用）：

```
:2015                    # 主机: (任何), 端口: 2015
localhost                # 主机: localhost; 端口: 2015
localhost:8080           # 主机: localhost; 端口: 8080
example.com              # 主机: example.com; 端口: 80->443
http://example.com       # 主机: example.com; 端口: 80
https://example.com      # 主机: example.com; 端口: 80->443
http://example.com:1234  # 主机: example.com; 端口: 1234
https://example.com:80   # 错误! HTTPS on port 80
*.example.com            # 多个主机: *.example.com; 端口: 2015
example.com/foo/         # 主机: example.com; 端口: 80, 443; 路径: /foo/
/foo/                    # 主机: (any), 端口: 2015, 路径: /foo/
```

站点地址必须是唯一的，站点的所有配置必须分组在同一个定义中。

## 路径匹配 <span id="path-matching"></span>
有一些指令接受一个指定要匹配的基本路径的参数，基本路径是前缀，如果URL以基本路径开头，它将是一个匹配。 例如，/foo的基本路径将匹配/foo，/foo.html，/foobar和/foo/bar.html的请求，如果你想限制一个基本路径匹配一个特定的目录，然后用/foo/，这将不匹配/ foobar的正斜杠后缀。

## 指令 <span id="directives"></span>
大多数指令调用了一层中间件，中间件是应用程序中处理HTTP请求的小层，并且做的很好。中间件在启动时被链接在一起（预编译，如果你愿意），只有从Caddyfile调用的中间件处理程序将被链接起来，所以小的Caddyfiles是非常快速和高效的。

参数的语法因指令而异, 有些参数是必需要的，而其它的指令则没有，请参考每个指令的文档特征。

对于注册为插件的指令，文档页面将显示以服务器类型为前缀的指令名称，例如"http.realip"或"dns.dnssec"。 在Caddyfile中使用它们时，要删除前缀（"http."），前缀只是为了保证唯一的命名，但是没有在Caddyfile中使用。

## 占位符 <span id="placeholders"></span>
在某些情况下，指令可以接受占位符（可替换的值），这些是由大括号{}包围起来的单词，并且在请求时由HTTP服务器解释。 例如{query}或{> Referer}，把它们当作是变量，这些占位符与你在Caddyfiles中使用的环境变量没有任何关系，除了我们借鉴了熟悉的语法。