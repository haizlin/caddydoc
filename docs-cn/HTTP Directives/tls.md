# tls
tls配置HTTPS的连接，由于HTTPS是自动启用的，因此该指令只能用于有意覆盖默认设置，请谨慎使用，如果有的话。

Caddy supports SNI (Server Name Indication), so you can serve multiple HTTPS sites from the same port on your machine. In addition, Caddy implements OCSP stapling for all qualifying certificates. Caddy also automatically rotates all TLS session ticket keys periodically.
Caddy支持SNI（服务器名称指示），因此您可以从机器上的同一端口提供多个HTTPS站点。 此外，凯迪为所有合格证书实施OCSP装订。 凯蒂还会自动轮换所有TLS会话票据密钥。

The tls directive will ignore sites that are explicitly defined to be http:// or are on port 80. This allows you to use the tls directive in a server block that is shared with both HTTP and HTTPS sites.
tls指令将忽略明确定义为http：//或在端口80上的站点。这允许您在与HTTP和HTTPS站点共享的服务器块中使用tls指令。

If not all the hostnames are not known when starting the server, you can use the On-Demand TLS feature, which issues certificates during the TLS handshake rather than at startup.
如果在启动服务器时并不是所有的主机名都不知道，那么您可以使用按需TLS功能，该功能在TLS握手期间发出证书，而不是在启动时发出证书。

Caddy ships with sane defaults for cipher suites, curves, key types, and protocols. Their exact selection and ordering may change at any time with new releases. You probably do not need to change them yourself. Adjust the TLS configuration at your own risk.
Caddy对于密码套件，曲线，密钥类型和协议都有合理的默认值。 他们的确切选择和排序可能随着新版本而改变。 你可能不需要自己改变。 调整TLS配置自担风险。

## 语法
```
tls off
```
不推荐禁用站点的TLS，除非你有一个很好的理由。 关闭TLS，自动HTTPS也被禁用，因此默认端口（2015）将不会更改。

```
tls email
```
email is the email address to use with which to generate a certificate with a trusted CA. By providing an email here you will not be prompted when you run Caddy.
Although the above syntax is not needed to enable TLS, it allows you to specify the email address used for your CA account, instead of prompting for one or using another one from a previous run.

要使用Caddy自己的证书和密钥：
```
tls cert key
```
cert is the certificate file. If the certificate is signed by a CA, this certificate file should be a bundle: a concatenation of the server's certificate followed by the CA's certificate (root certificate usually not necessary).
key is the server's private key file which matches the certificate file.
You can use this directive multiple times to specify multiple certificate and key pairs.

Or to have Caddy generate and use an untrusted, self-signed certificate in memory:

tls self_signed
The above syntaxes use Caddy's default TLS settings with your own certificate and key or a self-signed certificate, which should be sufficient in most cases.

Advanced users may open a settings block for more control, optionally specifying their own certificate and key:

```
tls [cert key] {
    ca        uri
    protocols min max
    ciphers   ciphers...
    curves    curves...
    clients   [request|require|verify_if_given] clientcas...
    load      dir
    max_certs limit
    key_type  type
    dns       provider
    alpn      protos...
    must_staple
}
```
ca specifies the ACME-compatible Certificate Authority endpoint to request certificates from.
cert and key are the same as above.
protocols specifies the minimum and maximum protocol versions to support. See below for valid values. If min and max are the same, it need only be specified once.
ciphers is a list of space-separated ciphers that will be supported, overriding the defaults. If you list any, only the ones you specify will be allowed and preferred in the order given. See below for valid values.
curves is a list of space-separated curves that will be supported in the given order, overriding the defaults. Valid curves are listed below.
clients is a list of space-separated client root CAs used for verification during TLS client authentication. If used, clients will be asked to present their certificate by their browser, which will be verified against this list of client certificate authorities. A client will not be allowed to connect if their certificate was not signed by one of these root CAs. Note that this setting applies to the entire listener, not just a single site. You may modify the strictness of client authentication using one of the keywords before the list of client CAs:
request merely asks a client to provide a certificate, but will not fail if none is given or if an invalid one is presented.
require requires a client certificate, but will not verify it.
verify_if_given will not fail if none is presented, but reject all that do not pass verification.
The default, if no flag is set but a CA file found, is to do both: to require client certificates and validate them.
load is a directory from which to load certificates and keys. The entire directory and its subfolders will be walked in search of .pem files. Each .pem file must contain the PEM-encoded certificate (chain) and key blocks, concatenated together.
max_certs enables On-Demand TLS, which obtains certificates during the first handshake requiring it rather than at startup. This is NOT recommended if the full hostname is given in the Caddyfile and known at startup! The limit value restricts the number of certificates allowed to be issued on demand (during TLS handshake) for this site. It must be a positive integer. This value gets reset after the process exits (but is preserved through reloads).
key_type is the type of key to use when generating keys for certificates (only applies to managed or TLS or self-signed certificates). Valid values are rsa2048, rsa4096, rsa8192, p256, and p384. Default is currently rsa2048.
dns is the name of a DNS provider; specifying it enables the DNS challenge. Note that you need to give credentials for it to work.
alpn is a list of space-separated protocols to use for Application-Layer Protocol Negotiation (ALPN). For HTTPS servers, HTTP versions can be enabled/disabled with this setting. Default is h2 http/1.1.
must_staple enables Must-Staple for managed certificates. Use with care.

## Protocols
The following protocols are supported, in descending order of preference:

tls1.2 (default max)
tls1.1 (default min)
tls1.0
Note that setting the minimum protocol version too high will restrict the clients which are able to connect, but with the benefit of better privacy.

Supported protocols and default protocol versions may be changed at any time.

Cipher Suites
The following cipher suites are currently supported:

ECDHE-ECDSA-AES256-GCM-SHA384
ECDHE-RSA-AES256-GCM-SHA384
ECDHE-ECDSA-AES128-GCM-SHA256
ECDHE-RSA-AES128-GCM-SHA256
ECDHE-ECDSA-WITH-CHACHA20-POLY1305
ECDHE-RSA-WITH-CHACHA20-POLY1305
ECDHE-RSA-AES256-CBC-SHA
ECDHE-RSA-AES128-CBC-SHA
ECDHE-ECDSA-AES256-CBC-SHA
ECDHE-ECDSA-AES128-CBC-SHA
RSA-AES256-CBC-SHA
RSA-AES128-CBC-SHA
ECDHE-RSA-3DES-EDE-CBC-SHA
RSA-3DES-EDE-CBC-SHA
Note: The HTTP/2 spec blacklists over 275 cipher suites for security reasons. Unless you know what you're doing, it's best to accept the default cipher suite settings.
Cipher suites may be added to or removed from Caddy at any time. Similarly, the default cipher suites may be changed at any time.

## Curves
The following curves are supported for EC cipher suites:

X25519
p256
p384
p521

## 例子
Remember, TLS is enabled by default, and this directive is not usually needed! These examples are for advanced users who manage certificates manually or need custom settings.

Serve with HTTPS using a certificate and private key located one folder up:

```
tls ../cert.pem ../key.pem
```

Obtain certificates during TLS handshakes as needed, with a hard limit of 10 new certificates:

```
tls {
    max_certs 10
}
```
Load all certificates and keys from .pem files found in /www/certificates:

```
tls {
    load /www/certificates
}
```
Serve a site with a self-signed certificate in memory (untrusted by browsers, but convenient for local development):

```
tls self_signed
```