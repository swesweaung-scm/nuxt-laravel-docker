###########[nginx]#############
# FROM nginx:1.17

# WORKDIR /var/www
# COPY . .
# COPY default.conf.aws /etc/nginx/conf.d/default.conf

# EXPOSE 80

###########[PHP]#############
FROM php:8.1.0alpha3-fpm-alpine

RUN apk add --update build-base curl git zip unzip vim zlib-dev libzip libzip-dev mysql-client

RUN apk add libxml2-dev curl-dev $PHPIZE_DEPS

RUN apk add wkhtmltopdf

RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install composer
RUN set -x \
   && php -r "copy('https://getcomposer.org/installer', '/tmp/composer-setup.php');" \
   && php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') !== trim(file_get_contents('https://composer.github.io/installer.sig'))) { echo 'ERROR: Invalid installer signature' . PHP_EOL; unlink('/tmp/composer-setup.php'); exit(1); }" \
   && php /tmp/composer-setup.php --version=1.10.13 --install-dir=/usr/local/bin --filename=composer   \
   && rm -rf /tmp/composer-setup.php

# Set composer environment
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www
COPY . .

# Install laravel installer
RUN set -x \
  && composer config -g repositories.packagist composer https://packagist.jp \
  && composer global require hirak/prestissimo \
  && composer global require "laravel/installer" \
  && composer install

# fix chmod
RUN set -x \
  && chmod -R 777 storage/

EXPOSE 9000
