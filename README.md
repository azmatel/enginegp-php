# EngineGP PHP Docker Image

Docker images for PHP 8.4 FPM used in the EngineGP project.

## Images

| Tag | Description |
|-----|-------------|
| `ghcr.io/azmatel/enginegp-php/php:8.4` | Production |
| `ghcr.io/azmatel/enginegp-php/php:8.4-dev` | Development (with Xdebug) |

## PHP Extensions

- `pdo_mysql`, `mysqli` — MySQL database support
- `gd` — image processing (JPEG, WebP, FreeType)
- `imagick` — ImageMagick
- `zip`, `curl`, `gmp`, `bz2`, `xml`, `mbstring`, `exif`
- `memcached` — Memcached caching
- `xdebug` — only in `8.4-dev`

## Quick Start

### Production

```yaml
services:
  php:
    image: ghcr.io/azmatel/enginegp-php/php:8.4
    volumes:
      - ./app:/var/www/enginegp
    ports:
      - "9000:9000"
```

### Development

```yaml
services:
  php:
    image: ghcr.io/azmatel/enginegp-php/php:8.4-dev
    volumes:
      - ./app:/var/www/enginegp
    ports:
      - "9000:9000"
    environment:
      XDEBUG_MODE: debug
      XDEBUG_CONFIG: client_host=host.docker.internal
```

## Composer Dependencies

After starting the container, install project dependencies:

```bash
# Install all dependencies
docker compose exec php composer install

# Install without dev dependencies (production)
docker compose exec php composer install --no-dev --optimize-autoloader

# Add a new package
docker compose exec php composer require vendor/package
```

### Example docker-compose.yaml

```yaml
services:
  php:
    image: ghcr.io/azmatel/enginegp-php/php:8.4
    volumes:
      - ./app:/var/www/enginegp
    working_dir: /var/www/enginegp
    depends_on:
      - mysql

  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: enginegp
    volumes:
      - mysql_data:/var/lib/mysql

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./app:/var/www/enginegp
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php

volumes:
  mysql_data:
```

## Local Build

```bash
# Production
docker build -t enginegp-php:8.4 ./production

# Development
docker build -t enginegp-php:8.4-dev ./development
```

## CI/CD

On push to the `main` branch, both images are automatically built and published via GitHub Actions.

## Structure

```
├── production/
│   └── Dockerfile      # Without Xdebug
├── development/
│   └── Dockerfile      # With Xdebug
└── .github/
    └── workflows/
        └── docker.yml  # Automated build
```
