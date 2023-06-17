# nginx-proxy

An Nginx proxy container setup to handle SSL/TLS connects for proxied containers. Uses LetsEncrypt certs.

## .env

You'll need to create the following `.env` file:

```bash
DEFAULT_LETSENCRYPT_EMAIL=
```

## Run

```bash
docker compose up -d
```

## Add A New Proxied Container

In order to add a new proxied container you need to add the following to it's docker-compose.yml:

Expose the port that Nginx should use:

Note: This can be ports `80` and/or `443` even though Nginx is binding to these on the host. This is because we're using a Docker Bridge network and not the hosts networking stack.

```yaml
services:
  <app>:
    expose:
      - <port>
```

Set the required Nginx env values:

```yaml
environment:
  # nginx-proxy
  - VIRTUAL_HOST="sub.domain.tld"
  - VIRTUAL_PORT=<port> # The same port you exposed above.
```

Set the required LetsEncrypt env values:

```yaml
environment:
  # acme-companion
  - LETSENCRYPT_HOST="sub.domain.tld"
  - LETSENCRYPT_EMAIL="your.email@email.com"
```

Add the `nginx-proxy` network:

Note: This only needs to be added to the application the proxy will be directly forwarding requests to. Your DB etc doesn't need to be able to speak to this network (unless it's being proxied...).

```yaml
services:
  <app>:
    networks:
      - nginx-proxy
networks:
  nginx-proxy:
    name: nginx-proxy
    external: true
```

Add a `depends_on` for the proxy so your service isn't started before the proxy is ready:

```yaml
services:
  <app>:
    depends_on:
      - nginx-proxy
```
