# http.filter  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.filter** plugin when you download Caddy.
Provides a directive to filter response bodies.

his could be useful to modify static HTML files to add (for example) Google Analytics source code to it.

## Examples
**Replace in every text file Foo with Bar.**
```
filter rule {
    path .*\.txt
    search_pattern Foo
    replacement Bar
}
```

**Insert server name in every HTML page.**
```
filter rule {
    content_type text/html.*
    search_pattern Server
    replacement "This site was provided by {response_header_Server}"
}
```

**Add Google Analytics to every HTML page from a file.**
```
filter rule {
    path .*\.html
    search_pattern </title>
    replacement @googleAnalyticsSnippet.html
}
```