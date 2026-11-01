## 👋 Welcome to yourls 🚀

Self-hosted URL shortener with bookmarking and API

## 📋 Description

YOURLS (Your Own URL Shortener) is a PHP URL shortener that doubles as a
bookmark manager. This stack runs the official YOURLS container with a MariaDB
backend behind the host nginx proxy.

## 🚀 Services

- **app**: yourls:latest

### Infrastructure Components

- **yourls-db**: Mariadb database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/yourls/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/yourls" ~/.local/srv/docker/yourls
cd ~/.local/srv/docker/yourls
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install yourls
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
DB_USER_NAME=yourls
DB_USER_PASS=replace-with-a-random-password
DB_ADMIN_PASS=replace-with-a-random-password
APP_ADMIN_USER=administrator
APP_ADMIN_PASS=replace-with-a-random-password
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64395` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

YOURLS also needs its loader endpoint (`/yourls-loader.php`) reachable by
short links, so the proxy must not buffer or rewrite paths under that prefix.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64395

## 📂 Volumes

- `./volumes/db/mariadb/yourls` - Database data

## 🔐 Security

- Change `APP_ADMIN_PASS` and both database passwords before the first start
- Use HTTPS via the reverse proxy in production, `YOURLS_SITE` already assumes
  it
- The database is reachable only on the internal project network
- The YOURLS API is unauthenticated by default, restrict it at the proxy if
  you expose it publicly
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
