# The HTTP Caddyfile
This page documents how the HTTP server uses the Caddyfile. If you haven't already, take the Caddyfile tutorial or read up on the Caddyfile syntax first.

**Topics**
1. Site Addresses
2. Path Matching
3. Directives
4. Placeholders

## Site Addresses
The HTTP server uses site addresses for labels. Addresses are specified in the form  scheme://host:port/path, where all but one are optional.

The host portion is usually localhost or the domain name. The default port is 2015 (unless the site qualifies for automatic HTTPS, in which case it's changed to 443). The scheme portion is another way to specify a port. Valid schemes are "http" or "https" which represent, respectively, ports 80 and 443. If both a scheme and port are specified, the port takes precedence. For example (this table assumes automatic HTTPS is applied where it qualifies):

```
:2015                    # Host: (any), Port: 2015
localhost                # Host: localhost; Port: 2015
localhost:8080           # Host: localhost; Port: 8080
example.com              # Host: example.com; Ports: 80->443
http://example.com       # Host: example.com; Port: 80
https://example.com      # Host: example.com; Ports: 80->443
http://example.com:1234  # Host: example.com; Port: 1234
https://example.com:80   # Error! HTTPS on port 80
*.example.com            # Hosts: *.example.com; Port: 2015
example.com/foo/         # Host: example.com; Ports: 80, 443; Path: /foo/
/foo/                    # Host: (any), Port: 2015, Path: /foo/
```

Site addresses must be unique; all configuration for a site must be grouped in the same definition.

## Path Matching
Some directives accept an argument that specifies a base path to match. A base path is a prefix. If the URL starts with the base path, it will be a match. For example, a base path of  /foo will match requests to /foo, /foo.html, /foobar, and /foo/bar.html. If you want to limit a base path to match a specific directory only, then suffix it with a forward slash like  /foo/, which will not match /foobar.

## Directives
Most directives invoke a layer of middleware. Middleware is a small layer in the application that handles HTTP requests and does one thing really well. Middleware are chained together (pre-compiled, if you will) at startup. Only middleware handlers which are invoked from the Caddyfile will be chained in, so small Caddyfiles are very fast and efficient.

The syntax of arguments varies from directive to directive. Some have required arguments, others don't. Consult the documentation of each directive for their signatures.

For directives which are registered as plugins, the documentation pages will show the directive name prefixed with the server type, for example, "http.realip" or "dns.dnssec". When using them in the Caddyfile, drop the prefix ("http."). The prefix is just to assure unique naming, but is not used in the Caddyfile.

## Placeholders
In some cases, directives will accept placeholders (replaceable values). These are words that are enclosed by curly braces { } and interpreted by the HTTP server at request-time. For example, {query} or {>Referer}. Think of them like variables. These placeholders have no relationship to the environment variables you can use in Caddyfiles, except we borrowed the syntax for familiarity.