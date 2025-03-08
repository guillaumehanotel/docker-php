FROM guillaumehanotel/php:8.4-prod-latest

# Switch to root for installation operations
USER root

# Install development dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    autoconf \
    gcc \
    g++ \
    make \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Install Xdebug for debugging
RUN pecl install xdebug --no-cache \
    && docker-php-ext-enable xdebug

# Install Laravel Installer globally
RUN composer global require laravel/installer \
    && echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> /root/.zshrc

# Créer un répertoire pour les logs
RUN mkdir -p /var/log/php \
    && touch /var/log/php/error.log \
    && touch /var/log/xdebug.log \
    && chmod 777 /var/log/php/error.log \
    && chmod 777 /var/log/xdebug.log

# Copier le fichier php-dev.ini
COPY php-dev.ini /usr/local/etc/php/conf.d/zz-php-dev.ini

# Add global Composer binaries to PATH for laravel user
RUN echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> /home/laravel/.zshrc

# Set permissions for Laravel user
RUN chown -R laravel:laravel /home/laravel

# Add Laravel-specific healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=30s --retries=3 \
    CMD php -v || exit 1

# Switch back to Laravel user
USER laravel

# Set shell to Zsh
SHELL ["/bin/zsh", "-c"]

# Expose port for PHP-FPM
EXPOSE 9000

# Default command
CMD ["php-fpm"]