# nextcloud-public

A Nextcloud container setup to run behind an Nginx proxy with LetsEncrypt certificates.

## .env

You'll need to create the following `.env` file:

```bash
NEXTCLOUD_HOST=
TRUSTED_DOMAIN=

ADMIN_USER=
ADMIN_PASSWORD=

MYSQL_PASSWORD=
MYSQL_DATABASE=
MYSQL_USER=

LETSENCRYPT_EMAIL=
```

## Run

> Note: Before starting Nextcloud, you need to start an instance of nginx-proxy.

```bash
docker compose up -d
```
