# tls.dns.vultr `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **tls.dns.vultr** plugin when you download Caddy.

Allows you to obtain certificates using DNS records for domains managed with Vultr.

## Examples
**Usage**
```
tls {
    dns vultr
}
```
Configure this in your tls directive.