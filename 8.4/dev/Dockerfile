FROM guillaumehanotel/php:8.4-prod-latest

# Installer les dépendances et outils de développement
RUN apt-get update && apt-get install -y \
    git \
    wget \
    nano \
    sudo \
    ffmpeg \
    supervisor \
    default-mysql-client \
    zsh \
    cron \
    libpq-dev \
    libc-client-dev \
    libkrb5-dev \
    libmagickwand-dev \
    && rm -rf /var/lib/apt/lists/*

# Configurer et installer les extensions supplémentaires
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) \
    pdo_pgsql \
    intl \
    imap

# Installer Redis
RUN pecl install redis \
    && docker-php-ext-enable redis

# Installer Imagick
RUN pecl install imagick \
    && docker-php-ext-enable imagick

# Installer Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

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

# Copier le fichier php-dev.ini
COPY php-dev.ini /usr/local/etc/php/conf.d/zz-php-dev.ini
