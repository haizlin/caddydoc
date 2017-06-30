# tls.dns.dyn `PLUGIN`
This feature does not come with Caddy by default. To get it, select the **tls.dns.dyn** plugin when you download Caddy.
Allows you to obtain certificates using DNS records for domains managed with Dyn.

## Examples
**Usage**
```
tls {
    dns dyn
}
```
Configure this in your tls directive.