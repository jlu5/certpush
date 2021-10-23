# certpush

Certpush is a utility that generates Let's Encrypt TLS certificates for DNS round robins. It uses rootless certbot with DNS validation as its backend, creating a unique certificate for every server. Each certificate is only valid for that server's name(s) plus configured round robin addresses, allowing granular certificate revocation / removal compared to sharing certificates between servers.

Certpush was originally designed to provide TLS to IRC round robins, but has been extended to support shared DNS names in general (e.g. websites backed by anycast or GeoDNS).

An example configuration lives in `certpush/certpush.config.sh.example`.

```
$ ./certpush.sh
Usage:
./certpush.sh newserver [server name] - generates a certificate for the given server
./certpush.sh renew - renews all known certificate and pushes them to servers
./certpush.sh push [server name] - pushes the certificate for the given server via SFTP
./certpush.sh runcmd [command] [args] - run arbitrary certbot commands under certpush's config directories
```

## Cron job

As certpush uses a custom folder for the certbot path, you should add `certpush.sh renew` to a cron job or a similar service to ensure that certificates are kept up to date.

Example (weekly at 9 AM):

```
0 9 * * 1 /path/to/your/certpush.sh renew
```
