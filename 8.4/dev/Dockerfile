FROM php:8.4-fpm

# Installer les dépendances et outils de développement
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    curl \
    wget \
    nano \
    sudo \
    ffmpeg \
    supervisor \
    default-mysql-client \
    zsh \
    cron \
    libzip-dev \
    libonig-dev \
    libxml2-dev \
    libpq-dev \
    libc-client-dev \
    libkrb5-dev \
    libmagickwand-dev \
    && rm -rf /var/lib/apt/lists/*

# Configurer et installer les extensions PHP
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) \
    gd \
    pdo_mysql \
    mysqli \
    pdo_pgsql \
    zip \
    exif \
    mbstring \
    xml \
    intl \
    imap \
    opcache

# Installer Redis
RUN pecl install redis \
    && docker-php-ext-enable redis

# Installer Imagick
RUN pecl install imagick \
    && docker-php-ext-enable imagick

# Installer Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Configurer Xdebug
RUN echo "xdebug.mode=develop,debug,coverage" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.log=/var/log/xdebug.log" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.discover_client_host=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Installer Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installer Oh My Zsh pour un meilleur environnement de développement
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# Créer un répertoire pour les logs
RUN mkdir -p /var/log/php \
    && touch /var/log/php/error.log \
    && touch /var/log/xdebug.log \
    && chmod 777 /var/log/php/error.log \
    && chmod 777 /var/log/xdebug.log

# Copier le php.ini de production
COPY --from=guillaumehanotel/php:8.4-prod-latest /usr/local/etc/php/php.ini /usr/local/etc/php/php.ini

# Set working directory
WORKDIR /var/www/html

# Expose port 9000
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm"]
