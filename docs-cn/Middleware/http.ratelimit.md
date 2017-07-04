# http.ratelimit  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.ratelimit** plugin when you download Caddy.
ratelimit is used to limit the request processing rate based on client's IP address. Excessive requests will be terminated with an error 429 (Too Many Requests).

## Examples
**For single resource:**
```
ratelimit path rate burst unit
```
path is the file or directory to apply rate limit; rate is the limited request in every time unit (r/s, r/m, r/h) (e.g. 1); burst is the maximum burst size client can exceed; burst >= rate (e.g. 2); unit is the time interval (currently support: second, minute, hour).

**For multiple resources:**
```
ratelimit rate burst unit {
    resources
}
```
resources is a list of files/directories to apply rate limit, one per line.

**Limit clients to 2 requests per second (bursts of 3) to any resources in /r:**
```
ratelimit /r 2 3 second
```

**For the listed paths, limit clients to 2 requests per minute (bursts of 2) and always ignore `/dir/app.js`:**
```
ratelimit 2 2 minute {
    /foo.html
    /dir
    ^/dir/app.js
}
```