FROM php:8.2-fpm

LABEL maintainer="Guillaume HANOTEL"

# Timezone
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Copy PHP Conf
COPY php.ini /usr/local/etc/php/php.ini

# Install basic packages
RUN apt-get update && apt-get install -y \
    nano \
    ffmpeg \
    git \
    supervisor \
    wget \
    sudo \
    default-mysql-client \
    zsh \
    cron

# Install dev packages
RUN apt-get update & apt-get install -y  \
    $PHPIZE_DEPS \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libzip-dev  \
    libpng-dev  \
    libjpeg-dev  \
    libc-client-dev \
    libkrb5-dev \
    libmagickwand-dev --no-install-recommends \
    libxml2-dev  \
    libcurl4-openssl-dev  \
    libicu-dev

RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg

# Install php extensions
RUN docker-php-ext-install -j$(nproc) \
    pdo \
    pdo_mysql \
    curl \
    imap \
    gd \
    zip \
    pcntl \
    dom \
    opcache

# Support de redis
RUN pecl install redis && docker-php-ext-enable redis
# Support de apcu
RUN pecl install apcu && docker-php-ext-enable apcu
# Imagick
RUN pecl install imagick && docker-php-ext-enable imagick

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
