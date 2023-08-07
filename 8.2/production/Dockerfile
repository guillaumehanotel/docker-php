FROM php:8.2-fpm

LABEL maintainer="Guillaume HANOTEL"

# Timezone
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Copy PHP Conf
COPY php.ini /usr/local/etc/php/php.ini

# Install basic packages and dev packages
RUN apt-get update && apt-get install -y \
    nano \
    ffmpeg \
    git \
    supervisor \
    wget \
    sudo \
    default-mysql-client \
    zsh \
    zip \
    cron \
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
    libicu-dev \
    unixodbc-dev \
 && rm -rf /var/lib/apt/lists/*

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

# Install ODBC PHP Extension
RUN set -ex; \
	docker-php-source extract; \
	{ \
		echo '# https://github.com/docker-library/php/issues/103#issuecomment-271413933'; \
		echo 'AC_DEFUN([PHP_ALWAYS_SHARED],[])dnl'; \
		echo; \
		cat /usr/src/php/ext/odbc/config.m4; \
	} > temp.m4; \
	mv temp.m4 /usr/src/php/ext/odbc/config.m4; \
	docker-php-ext-configure odbc --with-unixODBC=shared,/usr; \
	docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr; \
	docker-php-ext-install odbc pdo_odbc; \
	docker-php-source delete

# INSTALL COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer