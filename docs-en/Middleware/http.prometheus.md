# http.prometheus  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.prometheus** plugin when you download Caddy.
This directive enables prometheus metrics for Caddy.

## Examples
Enable metrics
prometheus
For each virtual host that you want to see metrics for.

It optionally takes an address where the metrics are exported, the default is localhost:9180. The metrics path is fixed to /metrics.

You'll need to put this module early in the chain, so that the duration histogram actually makes sense. I've put it at number 0.