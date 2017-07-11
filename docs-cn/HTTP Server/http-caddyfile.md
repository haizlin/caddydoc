# The HTTP Caddyfile
这个文档介绍了HTTP服务器如何使用Caddyfile，如果还没有使用过Caddy，请先看下[Caddy](../Using Caddy/tutorial/caddyfile)的教程或者阅读下[Caddy](/docs/caddyfile)的语法。

**主题**  
1. [站点地址](#addresses)  
2. [路径匹配](#path-matching)  
3. [指令](#directives)  
4. [占位符](#placeholders)  

## 站点地址 <span id="addresses"></span>
The HTTP server uses site addresses for labels. Addresses are specified in the form  scheme://host:port/path, where all but one are optional.

HTTP服务器使用站点地址作为标签，地址以表单scheme://host:port/path，其中除一个可选项以外。

The host portion is usually localhost or the domain name. The default port is 2015 (unless the site qualifies for automatic HTTPS, in which case it's changed to 443). The scheme portion is another way to specify a port. Valid schemes are "http" or "https" which represent, respectively, ports 80 and 443. If both a scheme and port are specified, the port takes precedence. For example (this table assumes automatic HTTPS is applied where it qualifies):

主机部分通常是localhost或域名，默认端口为2015（除非站点符合自动HTTPS的条件，在这种情况下，它将更改为443）。 方案部分是指定端口的另一种方式。 有效方案是分别表示端口80和443的“http”或“https”。如果指定了方案和端口，则端口优先。 例如（此表假定自动HTTPS在其符合条件的情况下应用）：

```
:2015                    # Host: (任何), 端口: 2015
localhost                # Host: localhost; 端口: 2015
localhost:8080           # Host: localhost; 端口: 8080
example.com              # Host: example.com; 端口: 80->443
http://example.com       # Host: example.com; 端口: 80
https://example.com      # Host: example.com; Ports: 80->443
http://example.com:1234  # Host: example.com; 端口: 1234
https://example.com:80   # Error! HTTPS on port 80
*.example.com            # Hosts: *.example.com; 端口: 2015
example.com/foo/         # Host: example.com; Ports: 80, 443; Path: /foo/
/foo/                    # Host: (any), 端口: 2015, Path: /foo/
```

站点地址必须是唯一的，站点的所有配置必须分组在同一个定义中。

## 路径匹配 <span id="path-matching"></span>
Some directives accept an argument that specifies a base path to match. A base path is a prefix. If the URL starts with the base path, it will be a match. For example, a base path of  /foo will match requests to /foo, /foo.html, /foobar, and /foo/bar.html. If you want to limit a base path to match a specific directory only, then suffix it with a forward slash like  /foo/, which will not match /foobar.

一些指令接受一个指定要匹配的基本路径的参数。 基本路径是前缀，如果URL以基本路径开头，它将是一个匹配。 例如，/ foo的基本路径将匹配/ foo，/foo.html，/ foobar和/foo/bar.html的请求。 如果你想限制一个基本路径匹配一个特定的目录，然后用/ foo /，这将不匹配/ foobar的正斜杠后缀。

## 指令 <span id="directives"></span>
Most directives invoke a layer of middleware. Middleware is a small layer in the application that handles HTTP requests and does one thing really well. Middleware are chained together (pre-compiled, if you will) at startup. Only middleware handlers which are invoked from the Caddyfile will be chained in, so small Caddyfiles are very fast and efficient.
大多数指令调用了一层中间件。 中间件是应用程序中处理HTTP请求的小层，并且做的很好。 中间件在启动时被链接在一起（预编译，如果你愿意）。 只有从Caddyfile调用的中间件处理程序将被链接起来，所以小的Caddyfiles非常快速和高效。

The syntax of arguments varies from directive to directive. Some have required arguments, others don't. Consult the documentation of each directive for their signatures.
参数的语法因指令而异, 有些需要论证，其他人则没有。 请参阅每个指令的文档的签名。

For directives which are registered as plugins, the documentation pages will show the directive name prefixed with the server type, for example, "http.realip" or "dns.dnssec". When using them in the Caddyfile, drop the prefix ("http."). The prefix is just to assure unique naming, but is not used in the Caddyfile.
对于注册为插件的指令，文档页面将显示以服务器类型为前缀的指令名称，例如“http.realip”或“dns.dnssec”。 在Caddyfile中使用它们时，删除前缀（“http”）。 前缀只是为了保证唯一的命名，但是没有在Caddyfile中使用。

## 占位符 <span id="placeholders"></span>
In some cases, directives will accept placeholders (replaceable values). These are words that are enclosed by curly braces { } and interpreted by the HTTP server at request-time. For example, {query} or {>Referer}. Think of them like variables. These placeholders have no relationship to the environment variables you can use in Caddyfiles, except we borrowed the syntax for familiarity.

在某些情况下，指令将接受占位符（可替换值）。 这些是由大括号{}包围的单词，并且在请求时由HTTP服务器解释。 例如{query}或{> Referer}。 想像他们像变量。 这些占位符与您可以在Caddyfiles中使用的环境变量没有任何关系，除非我们借鉴了熟悉的语法。