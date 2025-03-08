# Docker PHP Images

This repository contains Docker images for PHP development and production environments.

## Available PHP Versions

- PHP 8.2 (FPM)
- PHP 8.3 (FPM)
- PHP 8.4 (FPM)

Each version has both development and production variants.

## Image Structure

- **Base**: PHP-FPM with essential extensions
- **Production**: Optimized for production environments
- **Development**: Based on the production image with additional development tools (XDebug)

## Rebuilding and Publishing Images

When you need to update an image (e.g., adding extensions or changing configurations), follow these steps:

### Step 1: Build the versioned production image

```bash
# Navigate to the production directory for the PHP version
cd 8.x/production

# Build with specific version tag
docker build -t username/php:8.x-prod-x.y.z .
```

### Step 2: Push the versioned production image

```bash
# Login to Docker Hub (if not already logged in)
docker login

# Push the versioned image
docker push username/php:8.x-prod-x.y.z
```

### Step 3: Build and push the 'latest' production image

```bash
# Build the latest tag
docker build -t username/php:8.x-prod-latest .

# Push the latest tag
docker push username/php:8.x-prod-latest
```

### Step 4: Rebuild the development image

Since the development image is based on the production image, you need to rebuild it after updating the production image:

```bash
# Navigate to the development directory
cd ../dev

# Build with specific version tag
docker build -t username/php:8.x-dev-x.y.z .

# Push the versioned image
docker push username/php:8.x-dev-x.y.z

# Build and push the latest tag
docker build -t username/php:8.x-dev-latest .
docker push username/php:8.x-dev-latest
```

Replace `username` with your Docker Hub username, `8.x` with the PHP version, and `x.y.z` with your semantic version number.

## Included Tools

- nano
- ffmpeg
- git
- supervisor
- wget
- sudo
- default-mysql-client
- zsh
- cron

## PHP Extensions

- zip
- pdo
- pdo_pgsql
- pdo_mysql
- curl
- imap
- gd
- intl
- dom
- redis
- xdebug (dev images only)
- imagick
- opcache

## Usage Example

To use these images in your projects:

```yaml
# docker-compose.yml example
services:
  php:
    image: username/php:8.4-prod-latest
    volumes:
      - ./:/var/www/html
    working_dir: /var/www/html
```

For development environments:

```yaml
# docker-compose.yml example
services:
  php:
    image: username/php:8.4-dev-latest
    volumes:
      - ./:/var/www/html
    working_dir: /var/www/html
```
