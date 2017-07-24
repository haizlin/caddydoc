# http.timeouts
timeouts 针对Caddy的HTTP超时配置：

*  **Read:** 读取整个请求（包括正文）的最长持续时间。
*  **Read Header:** 允许读取请求标头的时间总和。
*  **Write:** 超时写入响应之前的最长持续时间。
*  **Idle:** 等待下次请求的最长时间（使用时保持存活）。

Timeouts 是面对错误或恶意的客户端时保持服务器连接的重要方式。

Because timeouts apply to an entire HTTP server which may serve multiple sites defined in your Caddyfile, the timeout values for each site will be reduced to their minimum values (with 0 or none being the lowest) across all sites that share that server. It's a good idea to keep your timeouts the same or just set them on one site to avoid confusion. A timeout set on one site will apply to all sites that share that server.
因为超时应用于可能服务于Caddyfile中定义的多个站点的整个HTTP服务器，所以每个站点的超时值将被减少到共享该服务器的所有站点的最小值（0或无最低）。 保持你的超时是一个好主意，或者只是将它们设置在一个站点上，以避免混淆。 在一个站点上设置的超时将应用于共享该服务器的所有站点。

## 语法
将所有超时设置为相同的值：
```
timeouts val
```
*  **val** 适用于所有超时的持续时间值（例如30s，2m30s，5m，1h），设置为0或无，以禁用以前启用的超时。

你还可以单独配置每个超时：
```
timeouts {
	read   val
	header val
	write  val
	idle   val
}
```

有效值为持续时间，如果先前已启用，则为0或禁用超时。

## 例子
设置所有超时为1分钟：
```
timeouts 1m
```

设置自定义read和write的超时时间：
```
timeouts {
	read  30s
	write 20s
}
```