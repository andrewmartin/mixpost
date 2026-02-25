# Mixpost on Railway

Self-hosted [Mixpost Lite](https://mixpost.app/) (open-source social media management) deployed on [Railway](https://railway.com/).

## Deploy to Railway

### 1. Create a new Railway project

Go to [railway.com](https://railway.com/) and create a new project.

### 2. Add the database services

From the project canvas, click **+ New** and add:

- **MySQL** — select the MySQL template
- **Redis** (or KeyDB) — select the Redis template

### 3. Deploy this repo

Click **+ New** → **GitHub Repo** → select this repository.

### 4. Configure environment variables

In the Mixpost service settings, add these **environment variables**:

| Variable | Value |
|---|---|
| `APP_NAME` | `Mixpost` |
| `APP_KEY` | Generate with: `echo "base64:$(openssl rand -base64 32)"` |
| `APP_URL` | `https://${{RAILWAY_PUBLIC_DOMAIN}}` |
| `APP_DOMAIN` | `${{RAILWAY_PUBLIC_DOMAIN}}` |
| `APP_DEBUG` | `false` |
| `DB_HOST` | `${{MySQL.MYSQLHOST}}` |
| `DB_PORT` | `${{MySQL.MYSQLPORT}}` |
| `DB_DATABASE` | `${{MySQL.MYSQLDATABASE}}` |
| `DB_USERNAME` | `${{MySQL.MYSQLUSER}}` |
| `DB_PASSWORD` | `${{MySQL.MYSQLPASSWORD}}` |
| `REDIS_HOST` | `${{Redis.REDISHOST}}` |
| `REDIS_PORT` | `${{Redis.REDISPORT}}` |
| `REDIS_PASSWORD` | `${{Redis.REDISPASSWORD}}` |
| `PORT` | `9000` |

> The `${{ServiceName.VAR}}` syntax is Railway's [reference variable](https://docs.railway.com/guides/variables#referencing-another-services-variable) format. It auto-resolves to the actual values from your MySQL and Redis services.

### 5. Expose the service

In the Mixpost service **Settings** → **Networking**, click **Generate Domain** to get a public Railway URL.

### 6. Login

Default credentials:

- **Email:** `admin@example.com`
- **Password:** `changeme`

**Change these immediately after first login.**

## Cost

Railway Hobby plan at **$5/mo** includes $5 of usage credits covering the app, MySQL, and Redis — all from the same credit pool. Typical Mixpost usage fits within $5-10/mo.

## Links

- [Mixpost Documentation](https://docs.mixpost.app/)
- [Railway Documentation](https://docs.railway.com/)
- [Mixpost GitHub](https://github.com/inovector/mixpost)
