FROM php:8.3-fpm

LABEL maintainer="Guillaume HANOTEL"

# Timezone
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Copy PHP Conf
COPY php.ini /usr/local/etc/php/php.ini


# Mise à jour des paquets
RUN apt-get update

# Installer les dépendances système requises
RUN apt-get install -y --no-install-recommends nano
RUN apt-get install -y --no-install-recommends ffmpeg
RUN apt-get install -y --no-install-recommends git
RUN apt-get install -y --no-install-recommends supervisor
RUN apt-get install -y --no-install-recommends wget
RUN apt-get install -y --no-install-recommends sudo
RUN apt-get install -y --no-install-recommends zsh
RUN apt-get install -y --no-install-recommends zip
RUN apt-get install -y --no-install-recommends cron

RUN apt-get install -y --no-install-recommends \
    build-essential \
    libtool \
    autoconf \
    automake \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libssl-dev


# Nettoyer le cache APT
RUN rm -rf /var/lib/apt/lists/*

# Configurer GD
RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Installer les extensions PHP
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install exif
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install gd
RUN docker-php-ext-install zip
RUN docker-php-ext-install opcache
RUN docker-php-ext-install mbstring

# Installer les extensions PECL : Redis et APCu
RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install apcu && docker-php-ext-enable apcu

# INSTALL COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
