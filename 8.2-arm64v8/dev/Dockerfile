FROM guillaumehanotel/php:8.2-arm64-prod-latest

ENV XDEBUG_MODE develop,debug,coverage
ENV XDEBUG_START_WITH_REQUEST trigger
ENV XDEBUG_DISCOVER_CLIENT_HOST true
ENV XDEBUG_IDEKEY PHPSTORM
ENV XDEBUG_REMOTE_CLIENT_HOST "host.docker.internal"
ENV XDEBUG_REMOTE_CLIENT_PORT 9100
ENV XDEBUG_MAX_NESTING_LEVEL 9999


#Install & Enable Xdebug
RUN pecl install xdebug \
    && echo "[XDebug]" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.mode = $XDEBUG_MODE" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.start_with_request = $XDEBUG_START_WITH_REQUEST" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.discover_client_host = $XDEBUG_DISCOVER_CLIENT_HOST" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.idekey = $XDEBUG_IDEKEY" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.client_host = $XDEBUG_REMOTE_CLIENT_HOST" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.client_port = $XDEBUG_REMOTE_CLIENT_PORT" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.max_nesting_level = $XDEBUG_MAX_NESTING_LEVEL" >> /usr/local/etc/php/php.ini \
    && docker-php-ext-enable xdebug