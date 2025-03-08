FROM php:8.4-fpm

LABEL maintainer="Guillaume HANOTEL"

# Environment variables for easier configuration
ENV TZ=Europe/Paris

# Set timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install system dependencies (alphabetically sorted within groups)
RUN apt-get update && apt-get install -y --no-install-recommends \
    # Runtime tools
    cron \
    ffmpeg \
    git \
    nano \
    supervisor \
    unzip \
    wget \
    zip \
    zsh \
    # Laravel-specific dependencies
    libfreetype6-dev \
    libjpeg-dev \
    libmagickwand-dev \
    libonig-dev \
    libpng-dev \
    libpq-dev \
    libsqlite3-dev \
    libssl-dev \
    libxml2-dev \
    libzip-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/*


# Configure and install PHP extensions required for Laravel
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
    bcmath \
    exif \
    fileinfo \
    gd \
    intl \
    mbstring \
    mysqli \
    opcache \
    pcntl \
    pdo \
    pdo_mysql \
    pdo_sqlite \
    xml \
    zip

# Install PECL extensions
RUN pecl install redis --no-cache && docker-php-ext-enable redis \
    && pecl install apcu --no-cache && docker-php-ext-enable apcu \
    && pecl install imagick --no-cache && docker-php-ext-enable imagick

# Install Composer from the official image
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Create required directories and set permissions
RUN mkdir -p /var/log/php /var/log/supervisor \
    && chmod -R 777 /var/log/php /var/log/supervisor

# Install Oh My Zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# Copy php.ini configuration
COPY php.ini /usr/local/etc/php/

# Create a non-root user for running PHP-FPM
ARG USER_ID=1000
ARG GROUP_ID=1000
RUN groupadd -g ${GROUP_ID} laravel && \
    useradd -u ${USER_ID} -g laravel -m laravel -s /bin/zsh

# Create Laravel storage volume
VOLUME ["/var/www/html/storage"]

# Set working directory
WORKDIR /var/www/html

# Set appropriate permissions for Laravel
RUN chown -R laravel:laravel /var/www/html

# Add Laravel-specific healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=30s --retries=3 \
    CMD php -v || exit 1

# Expose port for PHP-FPM
EXPOSE 9000

# Switch to non-root user
USER laravel

# Set shell to Zsh
SHELL ["/bin/zsh", "-c"]

# Default command
CMD ["php-fpm"]