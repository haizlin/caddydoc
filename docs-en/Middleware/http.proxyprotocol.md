# http.proxyprotocol  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.proxyprotocol** plugin when you download Caddy.
This directive enables PROXY protocol (v1) support to Caddy.

The PROXY PROTOCOL allows the client IP to be passed through a load balancer like those used in AWS or Google Cloud.

## Examples
**Enable from a local subnet and fixed IP**
```
proxyprotocol 10.22.0.0/16 10.23.0.1/32
```

Enables IPv4 traffic from the 10.22.0.0/16 subnet and 10.23.0.1 IP to send PROXY headers.

**Enable from any source**
```
proxyprotocol 0.0.0.0/0 ::/0
```
This enables any client to specify PROXY data. (not recommended for production)