# tls.dns.namecheap `PLUGIN`
This feature does not come with Caddy by default. To get it, select the **tls.dns.namecheap** plugin when you download Caddy.
Allows you to obtain certificates using DNS records for domains managed with NameCheap.

## Examples
**Usage**
```
tls {
    dns namecheap
}
```
Configure this in your tls directive.