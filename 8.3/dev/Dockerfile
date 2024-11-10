FROM guillaumehanotel/php:8.3-prod-latest

ENV XDEBUG_MODE develop,debug,coverage
ENV XDEBUG_START_WITH_REQUEST trigger
ENV XDEBUG_DISCOVER_CLIENT_HOST true
ENV XDEBUG_IDEKEY PHPSTORM
ENV XDEBUG_REMOTE_CLIENT_HOST "host.docker.internal"
ENV XDEBUG_REMOTE_CLIENT_PORT 9100
ENV XDEBUG_MAX_NESTING_LEVEL 9999

# Install build dependencies
RUN apt-get update && apt-get install -y \
    autoconf \
    gcc \
    g++ \
    make \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Install & Enable Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug
