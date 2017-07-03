# tls.dns.route53 `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **tls.dns.route53** plugin when you download Caddy.
Allows you to obtain certificates using DNS records for domains managed with Amazon Route53.

## Examples
**Usage**
```
tls {
    dns route53
}
```
Configure this in your tls directive.