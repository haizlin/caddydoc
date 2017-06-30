# dns `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the dns plugin when you download Caddy.
A DNS server plugin.

## Examples
**CoreDNS**
```
example.org:53 {
    whoami
    proxy . 8.8.8.8:53
}
```
See https://coredns.io for more documentation.