# nginx-proxy

An Nginx proxy container setup to handle SSL/TLS connections for proxied containers. Uses LetsEncrypt certs.

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

### expose

Expose the port that Nginx should use when forwarding requests.

> Note: This can be ports `80` and/or `443` even though Nginx is binding to these on the host. This is because we're using a Docker Bridge network and not the hosts networking stack.

```yaml
services:
  <app>:
    expose:
      - <port>
```

### Nginx env

```yaml
environment:
  # nginx-proxy
  - VIRTUAL_HOST="sub.domain.tld"
  - VIRTUAL_PORT=<port> # The same port you exposed above.
```

### LetsEncrypt env

```yaml
environment:
  # acme-companion
  - LETSENCRYPT_HOST="sub.domain.tld"
  - LETSENCRYPT_EMAIL="your.email@email.com"
```

### nginx-proxy Network

> Note: This only needs to be added to the application the proxy will be directly forwarding requests too. Your database etc likely doesn't need to be able to speak to this network.

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

### depends_on

```yaml
services:
  <app>:
    depends_on:
      - nginx-proxy
```
