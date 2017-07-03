# http.grpc  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.grpc** plugin when you download Caddy.
The Caddy grpc plugin proxies gRPC calls from clients to gRPC servies. It makes it possible for gRPC services to be consumed from normal gRPC clients or from browsers (using the gRPC-Web protocol).

## Examples
**Basic setup**
```
grpc.example.com 
grpc localhost:9090
```
The above will be a basic setup of the grpc plugin.

This will proxy any grpc requests to localhost:9090

**Setup with other directives**
```
grpc.example.com 
prometheus
log
grpc localhost:9090
```
This will proxy any grpc requests to localhost:9090 along with the log and prometheus directives