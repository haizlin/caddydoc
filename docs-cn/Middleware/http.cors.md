# http.cors  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.cors** plugin when you download Caddy.
Supports Cross Origin Resource Sharing headers

## Examples
**Simple usage**
```
cors
```
Allows all origins access to all resources

**Only allow certain origin domains**
```
cors / http://mytrusteddomain.tld http://myotherdomain.com
```
Only allow cross-origin requests from a few specific domains

**Full config**
```
cors / {
    origin            http://allowedSite.com
    origin            http://anotherSite.org https://anotherSite.org
    methods           POST,PUT
    allow_credentials false
    max_age           3600
    allowed_headers   X-Custom-Header,X-Foobar
    exposed_headers   X-Something-Special,SomethingElse
}
```
Shows examples of all available options