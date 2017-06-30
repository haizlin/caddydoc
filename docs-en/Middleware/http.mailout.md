# http.mailout  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.mailout** plugin when you download Caddy.
mailout - a SMTP client email middleware with PGP encryption. Post form data from your website to your defined endpoint and receive the posted data as nicely formatted or even encrypted email.

## Examples
Minimal configuration with PGP support
mailout /mailout {
        maillog         /var/log/caddy/mailout/
        errorlog        /var/log/caddy/mailout/

        cyr1ll@5chumach3r.fm    https://keybase.io/cyrill/key.asc

        to              cyr1ll@5chumach3r.fm
        subject         "Private Blog Contact Form"
        body            /var/www/webspace.com/email.txt

        username        "what_ever_your_username_is@gmail.com"
        password        "NowW3Hav3Th35alad"
        host            smtp.gmail.com
        port            587

        ratelimit_interval 24h
        ratelimit_capacity 100
}
A full working example which sends the PGP encrypted emails via GMail. The public PGP key for the receiver email address (cyr1ll@5chumach3r.fm) gets loaded from keybase.io. For more details please have a look on the project homepage.