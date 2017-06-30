hook.pluginloader PLUGIN
This feature does not come with Caddy by default. To get it, select the hook.pluginloader plugin when you download Caddy.
NOTE: plugin currently incompatible with build server. Do not download. Alternative instructions are on plugin website.

pluginloader is an experimental approach to dynamically load Caddy plugins without having to rebuild Caddy.

pluginloader only works on Linux and requires caddyplug for plugin management.

Examples
Add plugins
caddyplug install git cloudfare
This installs the http.git and tls.dns.cloudfare plugins.

Remove plugins
caddyplug uninstall git cloudfare
This removes http.git and tls.dns.cloudfare plugins.