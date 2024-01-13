FROM php:8.2-fpm

RUN docker-php-ext-install pdo pdo_mysql

# Install xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# allow super user - set this if you use Composer as a
# super user at all times like in docker containers
ENV COMPOSER_ALLOW_SUPERUSER=1

# obtain composer using multi-stage build
# https://docs.docker.com/build/building/multi-stage/

COPY --from=composer/composer:latest-bin /composer /usr/bin/composer

#Here, we are copying only composer.json and composer.lock (instead of copying the entire source)
# right before doing composer install.
# This is enough to take advantage of docker cache and composer install will
# be executed only when composer.json or composer.lock have indeed changed!-
# https://medium.com/@softius/faster-docker-builds-with-composer-install-b4d2b15d0fff
#COPY ./app/composer.* ./

# install

#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
#RUN composer self-update

# copy application files to the working directory
COPY ./app .



# Xdebug has different modes / functionalities. We can default to 'off' and set to 'debug'
# when we run docker compose up if we need it
#ENV XDEBUG_MODE=off

# Copy xdebug config file into container
#COPY ./docker/php/conf.d/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY ./docker/php/conf.d/xdebug.ini "${PHP_INI_DIR}/conf.d"



#COPY ./docker/php/conf.d/xdebug.ini "${PHP_INI_DIR}/conf.d"

COPY ./docker/php/conf.d/custom.ini /usr/local/etc/php/conf.d/custom.ini

# run composer dump-autoload --optimize
RUN composer dump-autoload --optimize