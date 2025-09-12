EdgeOne module for Caddy
===========================

This package contains a DNS provider module for [Caddy](https://github.com/caddyserver/caddy). It can be used to manage DNS records with Tencent Cloud (as is <https://www.tencentcloud.com> or <https://cloud.tencent.com>) accounts.

## Caddy module name

```
dns.providers.edgeone
```

## Config examples

To use this module for the ACME DNS challenge, [configure the ACME issuer in your Caddy JSON](https://caddyserver.com/docs/json/apps/tls/automation/policies/issuer/acme/) like so:

```json
{
  "module": "acme",
  "challenges": {
    "dns": {
      "provider": {
        "name": "edgeone",
        "secret_id":  "TENCENTCLOUD_SECRET_ID",
        "secret_key": "TENCENTCLOUD_SECRET_KEY"
      }
    }
  }
}
```

or with the Caddyfile:

```
# globally

acme_dns edgeone {
  secret_id {env.TENCENTCLOUD_SECRET_ID}
  secret_key {env.TENCENTCLOUD_SECRET_KEY}
}
```

```
# one site

tls {
  dns edgeone {
    secret_id {env.TENCENTCLOUD_SECRET_ID}
    secret_key {env.TENCENTCLOUD_SECRET_KEY}
  }
}
```

You can replace `{env.TENCENTCLOUD_SECRET_ID}`,`{env.TENCENTCLOUD_SECRET_KEY}` with the actual auth token in the `""` if you prefer to put it directly in your config instead of an environment variable.

## Recommended additional configuration

To improve DNS propagation handling, it is recommended to add the following settings to your EdgeOne DNS provider configuration:

https://caddyserver.com/docs/caddyfile/directives/tls#acme

```
# Maximum time to wait for DNS record propagation before timing out.
propagation_timeout 10m

# Initial delay before checking for DNS propagation.
propagation_delay 5m

# DNS resolvers to use for propagation checks (Google IPv6 DNS and Cloudflare DNS).
resolvers 2402:4e00:: 119.29.29.29
```

## Authenticating

See [the associated README in the libdns package](https://github.com/libdns/edgeone) for important information about credentials.
