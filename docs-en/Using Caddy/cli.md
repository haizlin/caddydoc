Command Line Interface
This page describes Caddy's command line interface. For a quick reference and to see default values, run Caddy with -help or -h, for example: caddy -h.

Remember that Caddy runs fine without any options. These flags simply give you more control over the process if needed.
Flags
-agree
Indicates that you have read and agree to the Let's Encrypt Subscriber Agreement. If this flag is not specified, it is possible that Caddy will prompt you to agree to terms during runtime. Thus, this flag is recommended in automated environments.

-ca
Base URL to the certificate authority's ACME server directory. Used for creating TLS certificates.

-catimeout
Change the ACME CA HTTP timeout. Not usually necessary unless your network experiences significant latency contacting the ACME CA server. In those cases, raising this value can help. Accepts a duration value; default is 10s.

-conf
The Caddyfile to use to configure Caddy. Must be a valid path to the file, either relative or absolute.

-cpu
CPU cap. Can be a percentage (e.g. "75%") or a number indicating how many cores to use (e.g. 3).

-disable-http-challenge
Disables the ACME HTTP challenge used for obtaining certificates.

-disable-tls-sni-challenge
Disables the ACME TLS-SNI challenge used for obtaining certificates.

-email
Email address to use for TLS certificate generation if not specified for a site in the Caddyfile. It is not required, but is strongly recommended so you can recover your account if you lose your private key. If an email is not available, Caddy may prompt you for an email address during runtime. This option is recommended in automated environments if not specified in the Caddyfile.

-grace
Duration of the graceful shutdown period. If you reload extremely frequently (multiple times per second), make this duration short. Syntax is same as Go's time.ParseDuration function (5s, 1m30s, etc).

-help or -h
Show basic flag help. Caddy will terminate after showing help; it will not serve sites.

-host
The default hostname or IP address to listen on.

-http-port
The port to use for HTTP (default 80). Changing this can have unintended consequences, be careful. The ACME HTTP challenge requires port 80.

-https-port
The port to use for HTTPS (default 443). Changing this can have unintended consequences, be careful. The ACME TLS-SNI challenge requires port 443.

-http2
HTTP/2 support. Disable it for the whole process by setting to false. To disable for specific sites, use the tls directive's "alpn" setting.

-log
Enable the process log. The value must be either the path to a log file, stdout, or stderr. Caddy will create the log file if it does not already exist. This file will be used to log information and errors that occur during runtime. The log file is rotated when it gets large, so it is safe to use for long-running processes.

-pidfile
The pidfile to write. Used with automated environments. Caddy will write a file containing the current process ID.

-plugins
Lists the plugins registered with Caddy. Caddy will terminate after printing; it will not serve sites.

-port
The default port to listen on.

-quic
Enables experimental QUIC support. See the QUIC wiki page for more details how to experiment with QUIC.

-quiet
Quiet mode. If quiet, Caddy will not print informational initialization output, only the addresses being served.

-revoke
Hostname for which to revoke the SSL certificate. Caddy will stop after revocation is complete; it will not serve sites if this option is used. The certificate must be under Caddy's management. Revocation is meant for compromised private keys only. Do not revoke a certificate to renew it.

-root
Path to the default site root from which to serve files.

-type
Change the server type. Default is http. If your Caddyfile is for another server type, use this option to tell it which server type to use.

-validate
Parses the Caddyfile and exits. If syntactically valid, a message will be printed to stdout and the process log (if any) and will exit with status 0. If not, an error will be returned with a non-zero exit status.

-version
Prints the version. It also prints build information if not from a tagged release. Caddy will terminate after printing; it does not serve sites if this option is used.

Signals
On POSIX-compliant systems, Caddy can be controlled with signals. Here we list them roughly in order from the most forceful action to most graceful.

TERM
Forcefully exits the process without executing shutdown hooks.

INT
Forcefully exits the process after executing shutdown hooks. This is the only "signal" that works on Windows (Ctrl+C). A second SIGINT forces immediate termination, even if shutdown hooks are still running.

HUP
Gracefully stops the server, but does not execute shutdown hooks.

QUIT
Gracefully stops the server after executing shutdown hooks.

USR1
Reloads the configuration file, then gracefully restarts the server.

Short Caddyfile
Caddy also accepts non-flag arguments, which are understood to be shorthand Caddyfile text. This is useful for quick, temporary server instances.

Each unflagged argument is a line in a Caddyfile that serves the default host and port. Remember to enclose the line in quotes if it contains spaces or other special characters.

For example, a server that lets you browse files on the default host and port:

$ caddy browse
To serve markdown files on-the-fly, instantly, on a custom port:

$ caddy -port 8080 markdown
All of the above, but with an access log:

$ caddy -port 8080 browse markdown "log access.log"
This shorthand feature is intended for quick, simple configurations only.

Pipe a Caddyfile
Advanced users may wish to pipe the contents of a Caddyfile into Caddy from programmed environments. If you pipe in the Caddyfile, you must use the -conf flag with a value of stdin - for example:

$ echo "localhost:1234" | caddy -conf stdin
Piping the Caddyfile is convenient when starting Caddy using a dynamically-generated Caddyfile from a parent process you have control over.

Warning: If you pipe in a Caddyfile, it will be impossible to read from stdin later in the program because the parent process must send EOF to close the pipe so Caddy can unblock and start serving. This will cause problems, for instance, if Caddy has to prompt you for an email address or agreement to terms. So when piping input, use flags to avoid the need for stdin later (e.g. the -email flag).
Environment Variables
Caddy recognizes certain environment variables.

HOME
The home folder. Caddy will create a .caddy folder here if using managed TLS (automatic HTTPS), and possibly persist other state here in the future or if configured to do so.

CADDYPATH
If set, Caddy will use this folder to store assets instead of the default $HOME/.caddy.

CASE_SENSITIVE_PATH
If 0 or false, Caddy will treat request paths in a case-insensitive manner when accessing assets on the file system or matching requests for middleware handlers. The default is 1/true (case-sensitive paths).