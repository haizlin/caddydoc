# hook.service `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the hook.service plugin when you download Caddy.
Always wanted to run Caddy as a service? Here's what you've been waiting for.

This plugin was coded by me and Henrique Dias, who is the main contributor for File Manager.

Notes: Notice that if you install the service with a name that is not the default's, you will need to specify it everytime you use one of the other commands using the flag -name.

## Examples
**Install service**
```
caddy -service install [-name optionalName]
```

**Uninstall service**
```
caddy -service uninstall [-name optionalName]
```

**Start service**
```
caddy -service start [-name optionalName]
```

**Stop service**
```
caddy -service stop [-name optionalName]
```

**Restart service**
```
caddy -service restart [-name optionalName]
```