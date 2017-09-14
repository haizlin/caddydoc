# http.websocket
websocket facilitates a WebSocket server/proxy. A command is executed when a new WebSocket connection is made and Caddy relays the client's connection to the command.
websocket有助于WebSocket server/proxy，当创建新的WebSocket连接时，执行命令，并且Caddy中继客户端与该命令的连接。

Any command can be executed as long as it takes input from stdin and writes to stdout, as this is how it will communicate with the WebSocket client. The command does not need to know that it is talking to a Web Socket client; just use stdin and stdout.
只要从stdin输入并写入stdout，任何命令都可以执行，因为它将与WebSocket客户端进行通信。 该命令不需要知道它正在与WebSocket客户端通信; 只需使用stdin和stdout。

Caddy will not make any effort to keep the backend process alive while a client is connected. It is the developer's responsibility to ensure the program does not terminate until the client is ready to close the connection or would be ready for it to terminate.
当客户端连接时，Caddy将不会努力保持后台进程的生效，开发人员有责任确保程序在客户端准备关闭连接或准备终止之前不会终止。

> 请注意，HTTP/2不支持协议升级，所以你必须禁用HTTP/2才能在安全连接上成功使用此这个指令。

## 语法

```
websocket [path] command
```
*  **path** 请求URL匹配的基本路径
*  **command** 要执行的命令

如果省略了路径，那么假设默认路径为/（表示所有请求）。

## 例子
简单的WebSockets echo服务器：
```
websocket /echo cat
```