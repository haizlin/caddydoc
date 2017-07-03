# http.hugo  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.hugo** plugin when you download Caddy.
hugo fills the gap between Hugo and the browser. Hugo is an easy and fast static website generator. This plugin fills the gap between Hugo and the end-user, providing you an web interface to manage the whole website.

Using this plugin, you won't need to have your own computer to edit posts, neither regenerate your static website, because you can do all of that just through your browser.

Requirements: you need to have the hugo executable in your PATH. You can download it from its official page.

## Examples
**Basic Usage**
```
root      public
basicauth /admin user pass
hugo
```

Display the generated website in public, protects the /admin URL with the defined credentials, and provides an interface at /admin with hugo plugin.

**Flags and site root**
```
root ./dist
hugo ./site {
    flag destination ../dist
}
```
Hugo website is at ./site and the flag destination is added to build the site to another directory.

**Change UI URL**
```
root  public
hugo  .   /cms
```
Changes the admin interface URL to /cms while the website files are located at . (default).