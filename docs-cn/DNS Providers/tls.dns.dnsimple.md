# tls.dns.dnsimple `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **tls.dns.dnsimple** plugin when you download Caddy.

Allows you to obtain certificates using DNS records for domains managed with DNSimple.

## Examples
**Usage** 
```
tls {
    dns dnsimple
}
```
Configure this in your tls directive.