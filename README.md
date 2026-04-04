# Laravel Docker 🐳

A generic Docker environment for **Laravel 12** local development — updated for the latest PHP, MySQL, Nginx, Node.js, and Redis.

## Stack

| Service | Image | Version |
|---------|-------|---------|
| PHP-FPM | `php:8.4-fpm` | PHP 8.4 |
| Nginx   | `nginx:1.27-alpine` | 1.27 (Alpine) |
| MySQL   | `mysql:8.4` | 8.4 |
| Node.js | `node:22-alpine` | 22 LTS |
| Redis   | `redis:7.4-alpine` | 7.4 |
| Composer | `composer:2` | 2.x (bundled in PHP image) |

## Ports

| Service | Host port | Container port |
|---------|-----------|---------------|
| Nginx   | **8999**  | 80  |
| MySQL   | **3399**  | 3306|
| Redis   | **6399**  | 6379|

## Quick Start

### 1. Clone / use this template

```bash
git clone https://github.com/rakibdevs/laravel-docker.git
cd laravel-docker
```

### 2. Copy the env file

```bash
cp .env.example .env
```

### 3. Build Docker images

```bash
docker compose build
```

### 4. Start containers

```bash
docker compose up -d
```

### 5. Create a new Laravel 12 project

```bash
# Give write permission to the empty src/ directory
sudo chmod 777 src/

# Create project inside the PHP container
docker compose exec php composer create-project laravel/laravel .
```

### 6. Configure Laravel's `.env`

Edit `src/.env` and update these values:

```dotenv
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=1234

REDIS_HOST=redis
REDIS_PORT=6379
CACHE_STORE=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
```

### 7. Run post-install artisan commands

```bash
docker compose exec php php artisan key:generate
docker compose exec php php artisan storage:link
docker compose exec php chmod -R 777 storage bootstrap/cache
docker compose exec php php artisan migrate
```

### 8. Build front-end assets (Vite)

```bash
# One-time production build
docker compose run --rm node npm run build

# Or watch mode during development
docker compose run --rm node npm run dev
```

### 9. Open in browser

👉 http://localhost:8999

---

## Common Commands

```bash
# Run Artisan commands
docker compose exec php php artisan <command>

# Run Composer commands
docker compose exec php composer <command>

# Run npm commands
docker compose run --rm node npm <command>

# Access MySQL CLI
docker compose exec mysql mysql -u root -p

# Access Redis CLI
docker compose exec redis redis-cli

# Tail logs
docker compose logs -f

# Stop all containers
docker compose down

# Stop and remove volumes (⚠️ deletes database)
docker compose down -v
```

---

## Changing PHP version

Edit `docker/php/Dockerfile` and update the `FROM` line:

```dockerfile
# PHP 8.2 (minimum for Laravel 12)
FROM php:8.2-fpm

# PHP 8.3
FROM php:8.3-fpm

# PHP 8.4 (recommended)
FROM php:8.4-fpm
```

Then rebuild:

```bash
docker compose build php
docker compose up -d
```

---

## Services Overview

### PHP (`docker/php/Dockerfile`)
Built from the official `php:8.4-fpm` image with all extensions required by Laravel 12:
`bcmath`, `ctype`, `curl`, `dom`, `exif`, `fileinfo`, `gd`, `gmp`, `intl`, `mbstring`, `opcache`, `pcntl`, `pdo_mysql`, `pdo_pgsql`, `redis`, `tokenizer`, `xml`, `zip`, and Composer 2.

### Nginx (`docker/nginx/nginx.conf`)
Configured for Laravel's public directory, PHP-FPM pass-through, static asset caching, gzip, and dotfile protection.

### MySQL (`mysql:8.4`)
The deprecated `--default-authentication-plugin` flag has been removed (it was dropped in MySQL 8.4). Includes a healthcheck so PHP waits for the database to be ready before starting.

### Node.js (`docker/node/Dockerfile`)
Node 22 LTS on Alpine with Python/make/g++ for native module builds. Used for Vite asset compilation.

### Redis (`redis:7.4-alpine`)
Lightweight Redis instance for cache, session, and queue drivers.

---

## Project Structure

```
laravel-docker/
├── docker/
│   ├── nginx/
│   │   └── nginx.conf        # Nginx server block
│   ├── node/
│   │   └── Dockerfile        # Node 22 LTS image
│   ├── php/
│   │   ├── Dockerfile        # PHP 8.4-FPM + extensions
│   │   ├── php.ini           # Custom PHP config
│   │   └── www.conf          # PHP-FPM pool config
│   └── database/             # MySQL data (git-ignored)
├── src/                      # Laravel application lives here
├── .env.example
├── .gitignore
├── docker-compose.yml
└── README.md
```