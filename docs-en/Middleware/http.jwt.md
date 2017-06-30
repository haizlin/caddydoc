# http.jwt  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.jwt** plugin when you download Caddy.
This middleware implements an authorization layer for Caddy based on JSON Web Tokens (JWT).

## Examples
**Basic Syntax**
```
jwt [path]
```
By default every resource under path will be secured using JWT validation. To specify a list of resources that need to be secured, use multiple declarations. Be sure to read the plugin documentation to properly configure your server to validate your tokens.

**Advanced Syntax**
```
jwt {
   path [path]
   redirect [location]
   allow [claim] [value]
   deny [claim] [value]
}
```
You can optionally use claim information to further control access to your routes. In a jwt block you can specify rules to allow or deny access based on the value of a claim. You can also specify a redirect URL so that invalid tokens can be sent to a login page. Check out the plugin documentation for more information about how to use the advanced syntax. It has more detailed examples of how the plugin will pass claims to your application.