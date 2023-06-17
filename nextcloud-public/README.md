# nextcloud-public

A Nextcloud container setup to run behind an Nginx proxy with LetsEncrypt certificate.

## .env

You'll need to create the following `.env` file:

```bash
NEXTCLOUD_HOST=
NEXTCLOUD_PORT=

MYSQL_PASSWORD=
MYSQL_DATABASE=
MYSQL_USER=

LETSENCRYPT_EMAIL=
DEFAULT_LETSENCRYPT_EMAIL=
```

## Run

TODO: If you split out the Nginx proxy (which you should) you'll need to add here about running that first.

```bash
docker compose up -d
```
