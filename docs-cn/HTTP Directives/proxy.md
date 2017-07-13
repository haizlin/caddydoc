# http.proxy
proxy facilitates both a basic reverse proxy and a robust load balancer. The proxy has support for multiple backends and adding custom headers. The load balancing features include multiple policies, health checks, and failovers. Caddy can also proxy WebSocket connections.

This middleware adds a placeholder that can be used in log formats: {upstream} - the name of the upstream host to which the request was proxied.

## 语法
在最基本的形式中，简单的反向代理使用这种语法：

```
proxy from to
```

*  **from** is the base path to match for the request to be proxied
to is the destination endpoint to proxy to (may include a port range)
However, advanced features including load balancing can be utilized with an expanded syntax:

```
proxy from to... {
	policy random | least_conn | round_robin | first | ip_hash | uri_hash
	fail_timeout duration
	max_fails integer
	max_conns integer
	try_duration duration
	try_interval duration
	health_check path
	health_check_port port
	health_check_interval interval_duration
	health_check_timeout timeout_duration
	header_upstream name value
	header_downstream name value
	keepalive number
	without prefix
	except ignored_paths...
	upstream to
	insecure_skip_verify
	preset
}
```

from is the base path to match for the request to be proxied.
to is the destination endpoint to proxy to. At least one is required, but multiple may be specified. If a scheme (http/https) is not specified, http is used. Unix sockets may also be used by prefixing "unix:".
policy is the load balancing policy to use; applies only with multiple backends. May be one of random, least_conn, round_robin, first, ip_hash, or uri_hash. Default is random.
fail_timeout specifies how long to remember a failed request to a backend. A timeout > 0 enables request failure counting and is required for load balancing between backends in case of failures. If the number of failed requests accumulates to the max_fails value, the host will be considered down and no requests will be routed to it until failed requests begin to be forgotten. By default, this is disabled (0s), meaning that failed requests will not be remembered and the backend will always be considered available. Must be a duration value (like "10s" or "1m").
max_fails is the number of failed requests within fail_timeout that are needed before considering a backend to be down. Not used if fail_timeout is 0. Must be at least 1. Default is 1.
max_conns is the maximum number of concurrent requests to each backend. 0 means no limit. When the limit is reached, additional requests will fail with Bad Gateway (502). Default is 0.
try_duration is how long to try selecting available upstream hosts for each request. By default, this retry is disabled ("0s"). Clients may hang for this long while the proxy tries to find an available upstream host. This value is only used if a request to the initially-selected upstream host fails.
try_interval is how long to wait between selecting another upstream host to handle a request. Default is 250ms. Only relevant when a request to an upstream host fails. Be aware that setting this to 0 with a non-zero try_duration can result in very tight looping and spin the CPU if all hosts stay down.
health_check will use path to check the health of each backend. If a backend returns a status code of 200-399, then that backend is considered healthy. If it doesn't, the backend is marked as unhealthy for at least health_check_interval and no requests are routed to it. If this option is not provided then health checks are disabled.
health_check_port will use port to perform the health check instead of the port provided for the upstream. This is useful if you use an internal port for debug purposes where your health check endpoint is hidden from public view.
health_check_interval specifies the time between each health check on unhealthy backends. The default interval is 30 seconds ("30s").
health_check_timeout sets a deadline for health check requests. If a health check does not respond within health_check_timeout, the health check is considered failed. The default is 60 seconds ("60s").
header_upstream sets headers to be passed to the backend. The field name is name and the value is value. This option can be specified multiple times for multiple headers, and dynamic values can also be inserted using request placeholders. By default, existing header fields will be replaced, but you can add/merge field values by prefixing the field name with a plus sign (+). You can remove fields by prefixing the header name with a minus sign (-) and leaving the value blank.
header_downstream modifies response headers coming back from the backend. It works the same way header_upstream does.
keepalive is the maximum number of idle connections to keep open to the backend. Enabled by default; set to 0 to disable keepalives. Set to a higher value on busy servers that are properly tuned.
without is a URL prefix to trim before proxying the request upstream. A request to /api/foo without /api, for example, will result in a proxy request to /foo.
except is a space-separated list of paths to exclude from proxying. Requests that match any of ignored_paths will be passed thru.
upstream specifies another backend. It may use a port range like ":8080-8085" if desired. It is often used multiple times when there are many backends to route to.
insecure_skip_verify overrides verification of the backend TLS certificate, essentially disabling security features over HTTPS.
preset is an optional shorthand way of configuring the proxy to meet certain conditions. See presets below.
Note that in order to do proper, redundant load balancing in the event of failures, you must set fail_timeout and try_duration to values > 0.

Everything after the first to is optional, including the block of properties enclosed by curly braces.

## Presets
The following presets are available:


websocket
Indicates this proxy is forwarding WebSocket connections. It is shorthand for:
```
header_upstream Connection {>Connection}
header_upstream Upgrade {>Upgrade}
```
HTTP/2 does not support protocol upgrade.

transparent
Passes thru host information from the original request as most backend apps would expect. Shorthand for:
```
header_upstream Host {host}
header_upstream X-Real-IP {remote}
header_upstream X-Forwarded-For {remote}
header_upstream X-Forwarded-Proto {scheme}
```

## Policies
There are three load balancing policies available:

random (default) - Randomly select a backend
least_conn - Select backend with the fewest active connections
round_robin - Select backend in round-robin fashion
first - Select the first available backend in the order they are defined in the Caddyfile
ip_hash - Select backend by hashing the request IP, distributing evenly over the hash space based on the total number of backends
uri_hash - Select backend by hashing the request URI, distributing evenly over the hash space based on the total number of backends

## 例子
将/api内的所有请求代理到后台系统：

```
proxy /api localhost:9005
```

Load-balance all requests between three backends (using random policy):

```
proxy / web1.local:80 web2.local:90 web3.local:100
```

Same as above, but round-robin style:

```
proxy / web1.local:80 web2.local:90 web3.local:100 {
	policy round_robin
}
```

With health checks and proxy headers to pass hostname, IP, and scheme upstream:

```
proxy / web1.local:80 web2.local:90 web3.local:100 {
	policy round_robin
	health_check /health
	transparent
}
```

代理WebSocket连接：
```
proxy /stream localhost:8080 {
	websocket
}
```

代理所有请求除了/static或/robots.txt之外的内容

```
proxy / backend:1234 {
	except /static /robots.txt
}
```