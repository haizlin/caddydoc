# 命令行界面
这个页面介绍了Caddy的命令行界面，要想快速查看命令和默认值，请使用`-help`或`-h`，例如：`caddy -h`。

> 请记住，Caddy没有任何选项也能很好的运行，如果需要的话，这些标识可以让你有更多的控制权。

## 标识
-agree  
Indicates that you have read and agree to the Let's Encrypt Subscriber Agreement. If this flag is not specified, it is possible that Caddy will prompt you to agree to terms during runtime. Thus, this flag is recommended in automated environments.
这表示你已阅读并同意Let‘s Encrypt用户协议。如果没有指定这个标识，那么在运行过程中，Caddy将提示你同意条款，因此推荐使用此标识。

*  **-ca**
证书颁发机构的ACME服务器的URL，用于创建TLS证书。

*  **-catimeout**
Change the ACME CA HTTP timeout. Not usually necessary unless your network experiences significant latency contacting the ACME CA server. In those cases, raising this value can help. Accepts a duration value; default is 10s.
更改ACME CA HTTP的超时时间，通常不需要，除非您的网络遇到与ACME CA服务器的重大延迟。 在这些情况下，提高此值可以帮助。 接受持续时间值; 默认为10秒。

*  **-conf**  
Caddy的配置文件Caddyfile必须是一个有效的路径，不论是相对路径还是绝对路径。

*  **-cpu**
CPU cap. Can be a percentage (e.g. "75%") or a number indicating how many cores to use (e.g. 3).

*  **-disable-http-challenge**
Disables the ACME HTTP challenge used for obtaining certificates.

*  **-disable-tls-sni-challenge**
Disables the ACME TLS-SNI challenge used for obtaining certificates.

*  **-email**
Email address to use for TLS certificate generation if not specified for a site in the Caddyfile. It is not required, but is strongly recommended so you can recover your account if you lose your private key. If an email is not available, Caddy may prompt you for an email address during runtime. This option is recommended in automated environments if not specified in the Caddyfile.

*  **-grace**
Duration of the graceful shutdown period. If you reload extremely frequently (multiple times per second), make this duration short. Syntax is same as Go's time.ParseDuration function (5s, 1m30s, etc).

*  **-help or -h**
显示最基本的帮助，Caddy在显示帮助后终止，它不是用于网站的服务。

*  **-host**
默认监听的主机名或者IP地址

*  **-http-port**
HTTP使用的端口（默认是80），如果改变这个可能会产生意想不到的后果，要小心。 这个ACME HTTP要求需要80端口。

*  **-https-port**
HTTPS使用的端口（默认是443），如果改变这个可能会产生意想不到的后果，要小心。 这个ACME TLS-SNI要求需要443端口。

*  **-http2**
HTTP/2 support. Disable it for the whole process by setting to false. To disable for specific sites, use the tls directive's "alpn" setting.

*  **-log**
Enable the process log. The value must be either the path to a log file, stdout, or stderr. Caddy will create the log file if it does not already exist. This file will be used to log information and errors that occur during runtime. The log file is rotated when it gets large, so it is safe to use for long-running processes.

*  **-pidfile**
The pidfile to write. Used with automated environments. Caddy will write a file containing the current process ID.

*  **-plugins**
Lists the plugins registered with Caddy. Caddy will terminate after printing; it will not serve sites.

*  **-port**
默认监听的端口。

*  **-quic**
Enables experimental QUIC support. See the QUIC wiki page for more details how to experiment with QUIC.

*  **-quiet**
Quiet mode. If quiet, Caddy will not print informational initialization output, only the addresses being served.

*  **-revoke**
Hostname for which to revoke the SSL certificate. Caddy will stop after revocation is complete; it will not serve sites if this option is used. The certificate must be under Caddy's management. Revocation is meant for compromised private keys only. Do not revoke a certificate to renew it.

*  **-root**
默认站点根路径，从它开始为文件提供服务。

*  **-type**
更改服务器类型，默认是http。如果你的Caddyfile使用了另外一种服务类型，请使用此选项来设置服务类型。

*  **-validate**
解析caddyfile并退出，如果语法正确的话，将会打印信息到stdout和进程日记（如果有的话）并以0的状态退出。否则，将返回一个非0退出状态的错误。

*  **-version**
Prints the version. It also prints build information if not from a tagged release. Caddy will terminate after printing; it does not serve sites if this option is used.  
打印版本号，如果没有标记的版本，它也会打印构建信息。 Caddy打印后终止; 如果使用此选项，它不会提供网站。

## 信号
On POSIX-compliant systems, Caddy can be controlled with signals. Here we list them roughly in order from the most forceful action to most graceful.

### TERM
Forcefully exits the process without executing shutdown hooks.

### INT
Forcefully exits the process after executing shutdown hooks. This is the only "signal" that works on Windows (Ctrl+C). A second SIGINT forces immediate termination, even if shutdown hooks are still running.

### HUP
Gracefully stops the server, but does not execute shutdown hooks.

### QUIT
Gracefully stops the server after executing shutdown hooks.

### USR1
Reloads the configuration file, then gracefully restarts the server.

## Short Caddyfile
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

## HOME
The home folder. Caddy will create a .caddy folder here if using managed TLS (automatic HTTPS), and possibly persist other state here in the future or if configured to do so.

## CADDYPATH
If set, Caddy will use this folder to store assets instead of the default $HOME/.caddy.

## CASE_SENSITIVE_PATH
If 0 or false, Caddy will treat request paths in a case-insensitive manner when accessing assets on the file system or matching requests for middleware handlers. The default is 1/true (case-sensitive paths).