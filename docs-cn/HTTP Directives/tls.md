# tls
tls configures HTTPS connections. Since HTTPS is enabled automatically, this directive should only be used to deliberately override default settings. Use with care, if at all.

Caddy supports SNI (Server Name Indication), so you can serve multiple HTTPS sites from the same port on your machine. In addition, Caddy implements OCSP stapling for all qualifying certificates. Caddy also automatically rotates all TLS session ticket keys periodically.

The tls directive will ignore sites that are explicitly defined to be http:// or are on port 80. This allows you to use the tls directive in a server block that is shared with both HTTP and HTTPS sites.

If not all the hostnames are not known when starting the server, you can use the On-Demand TLS feature, which issues certificates during the TLS handshake rather than at startup.

Caddy ships with sane defaults for cipher suites, curves, key types, and protocols. Their exact selection and ordering may change at any time with new releases. You probably do not need to change them yourself. Adjust the TLS configuration at your own risk.

## 语法
tls off
Disables TLS for the site. Not recommended unless you have a good reason. With TLS off, automatic HTTPS is also disabled, so the default port (2015) will not be changed.

tls email
email is the email address to use with which to generate a certificate with a trusted CA. By providing an email here you will not be prompted when you run Caddy.
Although the above syntax is not needed to enable TLS, it allows you to specify the email address used for your CA account, instead of prompting for one or using another one from a previous run.

To use Caddy with your own certificate and key:

tls cert key
cert is the certificate file. If the certificate is signed by a CA, this certificate file should be a bundle: a concatenation of the server's certificate followed by the CA's certificate (root certificate usually not necessary).
key is the server's private key file which matches the certificate file.
You can use this directive multiple times to specify multiple certificate and key pairs.

Or to have Caddy generate and use an untrusted, self-signed certificate in memory:

tls self_signed
The above syntaxes use Caddy's default TLS settings with your own certificate and key or a self-signed certificate, which should be sufficient in most cases.

Advanced users may open a settings block for more control, optionally specifying their own certificate and key:

tls [cert key] {
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
dns is the name of a DNS provider; specifying it enables the DNS challenge. Note that you need to give cred