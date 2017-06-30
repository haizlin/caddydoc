# tls.dns.digitalocean `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **tls.dns.digitalocean** plugin when you download Caddy.

Allows you to obtain certificates using DNS records for domains managed with DigitalOcean.

## Examples
**Usage** 
```
tls {
    dns digitalocean
}
```
Configure this in your tls directive.