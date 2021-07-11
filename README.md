# certpush

Certpush is a utility that generates Let's Encrypt TLS certificates for DNS round robins. It uses rootless certbot with DNS validation as its backend, creating a unique certificate for every server. Each certificate is only valid for that server's name(s) plus configured round robin addresses, allowing granular certificate revocation / removal compared to sharing certificates between servers.

Certpush was originally designed to provide TLS to IRC round robins, but has been extended to support shared DNS names in general (e.g. websites backed by anycast or GeoDNS).

Certpush uses the same scp-syncing mechanism as conf-sync below, and shares some config elements with it (target server addresses and paths).

An example configuration lives in `certpush/certpush.config.sh.example`.

```
$ ./certpush.sh
Usage:
./certpush.sh newserver [server name] - generates a certificate for the given server
./certpush.sh renew - renews all known certificate and pushes them to servers
./certpush.sh push [server name] - pushes the certificate for the given server via SFTP
./certpush.sh runcmd [command] [args] - run arbitrary certbot commands under certpush's config directories
```

`certpush.sh renew` can be added to a cron job to ensure that certificates are kept up to date.
