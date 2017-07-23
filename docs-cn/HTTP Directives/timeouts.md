# http.timeouts
timeouts configures Caddy's HTTP timeouts:
timeouts 针对Caddy的HTTP超时配置：

Read: Maximum duration for reading the entire request, including the body.
Read Header: The amount of time allowed to read request headers.
Write: The maximum duration before timing out writes of the response.
Idle: The maximum time to wait for the next request (when using keep-alive).
Timeouts are an important way to maintain server connectivity in the face of buggy or malicious clients.

Because timeouts apply to an entire HTTP server which may serve multiple sites defined in your Caddyfile, the timeout values for each site will be reduced to their minimum values (with 0 or none being the lowest) across all sites that share that server. It's a good idea to keep your timeouts the same or just set them on one site to avoid confusion. A timeout set on one site will apply to all sites that share that server.

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

Valid values are durations, or 0 or none to disable the timeout if it was previously enabled.

## 例子
Set all timeouts to 1 minute:

```
timeouts 1m
```

设置自定义读取和写入超时：

```
timeouts {
	read  30s
	write 20s
}
```