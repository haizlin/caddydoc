# tls.dns.linode `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **tls.dns.linode plugin** when you download Caddy.
Allows you to obtain certificates using DNS records for domains managed with Linode.

## Examples
**Usage**
```
tls {
    dns linode
}
```
Configure this in your tls directive.