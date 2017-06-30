# http.datadog  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.datadog** plugin when you download Caddy.
Datadog plugin allow Caddy HTTP/2 web server to send some metrics to Datadog.

This plugin is not only working with Datadog, it also compatible with all services using statsd.

## Examples
**Configuration**
```
example-b.com {
  datadog {
    statsd 127.0.0.1:8125
    tags tag1 tag2 tag3
  }
}
```