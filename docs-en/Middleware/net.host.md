# net.host  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **net.host** plugin when you download Caddy.
The caddy-net plugin is a TCP/UDP server type plugin to echo or proxy TCP/UDP traffic

## Examples
**Basic usage**
```
echo :22017 {
    host echo.example.com
}
```

proxy :12017 :22017 {
    host proxy.example.com
}
Above is a proposed Caddyfile to use.

The first server block will listen on port 22017 and echo any traffic back to caller

The second server block will listen on port 12017 and forward traffic to address :22017