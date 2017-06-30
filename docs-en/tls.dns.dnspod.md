# tls.dns.dnspod `PLUGIN`
This feature does not come with Caddy by default. To get it, select the **tls.dns.dnspod** plugin when you download Caddy.
Allows you to obtain certificates using DNS records for domains managed with DNSPod.

## Examples
**Usage** 
```
tls {
    dns dnspod
}
```
Configure this in your tls directive.