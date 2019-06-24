FROM composer:latest AS composer

FROM php:7.1-fpm
WORKDIR /var/www/html
RUN apt-get update && apt-get install -y \
  git zip apt-utils libfreetype6-dev libjpeg62-turbo-dev libpng-dev libxml2-dev libmcrypt-dev libpq-dev libmemcached-dev \
&& docker-php-ext-install -j$(nproc) soap bcmath mcrypt sockets intl pdo pdo_mysql \
&& pecl install memcached-3.0.4 \
&& docker-php-ext-enable memcached \
&& pecl install xdebug-2.6.0 \
&& docker-php-ext-enable xdebug \
&& echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.remote_connect_back = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.remote_port = 9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.scream = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.cli_color = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.show_local_vars = 1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

COPY php-dev-settings.ini $PHP_INI_DIR/conf.d/
COPY www.conf /usr/local/etc/php-fpm.d/www.conf
COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN usermod -u 1000 www-data
