# http.ipfilter  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.ipfilter** plugin when you download Caddy.
The ipfilter directive adds the ability to allow or block requests based on the client's IP address.

## Examples
Filter a specific IP or a CIDR range.
ipfilter / {
    rule block
    ip 70.1.128.0/19 2001:db8::/122 9.12.20.16
}
caddy will block any clients with IPs that fall into one of these two ranges 70.1.128.0/19 and 2001:db8::/122 , or a client that has an IP of 9.12.20.16 explicitly.

Filter clients based on their Country ISO Code
ipfilter / {
    rule allow
    database /data/GeoLite.mmdb
    country US JP
}
with that in your Caddyfile caddy will only serve users from the United States or Japan.

filtering with country codes requires a local copy of the Geo database, can be downloaded for free from MaxMind.

Define a block page
ipfilter / {
    rule allow
    blockpage default.html
    ip 55.3.4.20 2e80::20:f8ff:fe31:77cf
}
caddy will serve only these 2 IPs, eveyone else will get default.html.

Multiple paths
ipfilter /notglobal /secret {
    rule allow
    ip 84.235.124.4
}
Only serve 84.235.124.4 under /notglobal and /secret.

Multiple blocks
ipfilter / {
    rule allow
    ip 32.55.3.10
}

ipfilter /webhook {
    rule allow
    ip 192.168.1.0/24
}
You can use as many ipfilter blocks as you please, the above says: block everyone but  32.55.3.10, Unless it falls in 192.168.1.0/24 and requesting a path in /webhook.