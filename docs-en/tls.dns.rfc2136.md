# tls.dns.rfc2136 `PLUGIN`
This feature does not come with Caddy by default. To get it, select the **tls.dns.rfc2136** plugin when you download Caddy.
Allows you to obtain certificates using DNS records for domains managed with any RFC 2136 compliant DNS provider.

## Examples
**Usage**
```
tls {
    dns rfc2136
}
```
Configure this in your tls directive.