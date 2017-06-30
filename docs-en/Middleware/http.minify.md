# http.minify  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.minify** plugin when you download Caddy.
Minify static assets on-the-fly. Supports CSS, HTML, JS, JSON, SVG and XML.

## Examples
Simple syntax
minify
Minify all of the supported files of the website.

Complete Syntax
minify paths...  {
    if          a cond b
    if_op       [and|or]
    disable     [js|css|html|json|svg|xml]
    minifier    option value
}
paths are space separated file paths to minify. If nothing is specified, the whole website will be minified.
if specifies a condition. Multiple ifs are AND-ed together by default. a and b are any string and may use request placeholders. cond is the condition, with possible values explained in rewrite (which also has an if statement).
if_op specifies how the ifs are evaluated; the default is and.
disable is used to indicate which minifiers to disable. By default, they're all activated.
minifier sets value for option on that minifier. When the option is true or false, its omission is trated as true. To know the options read the full documentation.
Minify one path
minify /assets
Only minify the contents of /assets folder.

Minify everything except a folder
minify  {
    if {path} not_match ^(\/api).*
}
Minify the whole website except /api.