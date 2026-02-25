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

#### Cloudflare R2 Storage (recommended for media/video uploads)

| Variable | Value |
|---|---|
| `MIXPOST_DISK` | `s3` |
| `AWS_ACCESS_KEY_ID` | Your R2 API token Access Key ID |
| `AWS_SECRET_ACCESS_KEY` | Your R2 API token Secret Access Key |
| `AWS_DEFAULT_REGION` | `auto` |
| `AWS_BUCKET` | Your R2 bucket name |
| `AWS_ENDPOINT` | `https://<ACCOUNT_ID>.r2.cloudflarestorage.com` |
| `AWS_URL` | `https://pub-<hash>.r2.dev` (your R2 public bucket URL) |
| `AWS_USE_PATH_STYLE_ENDPOINT` | `true` |

> The `${{ServiceName.VAR}}` syntax is Railway's [reference variable](https://docs.railway.com/guides/variables#referencing-another-services-variable) format. It auto-resolves to the actual values from your MySQL and Redis services.

### 5. Expose the service

In the Mixpost service **Settings** → **Networking**, click **Generate Domain** to get a public Railway URL.

### 6. Login

Default credentials:

- **Email:** `admin@example.com`
- **Password:** `changeme`

**Change these immediately after first login.**

## Cloudflare R2 Setup

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/) → **R2 Object Storage**
2. **Create a bucket** (e.g. `mixpost-media`)
3. In bucket settings, enable **Public Access** (required — Instagram API needs public URLs for media)
4. Go to **R2 Overview** → **Manage R2 API Tokens** → **Create API token**
   - Permissions: **Object Read & Write**
   - Scope: your bucket
5. Copy the **Access Key ID**, **Secret Access Key**, and your **Account ID** into the Railway env vars above

**Free tier:** 10 GB storage, 10 million reads/mo, 1 million writes/mo, zero egress fees.

## Custom Domain (required for Facebook/Instagram)

1. In Railway, go to your Mixpost service → **Settings** → **Networking**
2. Add your custom domain (e.g. `mixpost.yourdomain.com`)
3. Add the CNAME record Railway provides to your DNS
4. Railway auto-provisions SSL via Let's Encrypt
5. Update `APP_URL` and `APP_DOMAIN` env vars to use your custom domain

## Cost

Railway Hobby plan at **$5/mo** includes $5 of usage credits covering the app, MySQL, and Redis — all from the same credit pool. Typical Mixpost usage fits within $5-10/mo.

## Links

- [Mixpost Documentation](https://docs.mixpost.app/)
- [Railway Documentation](https://docs.railway.com/)
- [Mixpost GitHub](https://github.com/inovector/mixpost)
