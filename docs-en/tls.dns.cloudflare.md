# tls.dns.cloudflare `PLUGIN`

> This feature does not come with Caddy by default. To get it, select the **tls.dns.cloudflare** plugin when you download Caddy.

Allows you to obtain certificates using DNS records for domains managed with Cloudflare.

## Examples
**Usage** 
``` 
tls {
    dns cloudflare
}
```
Use this from within your tls directive.