FROM php:8.1-fpm

COPY --from=composer:2.3.5 /usr/bin/composer /usr/bin/composer
COPY php.ini /etc/php/8.1/cli/conf.d/99-etc.ini

ADD docker-entrypoint.sh ./
RUN chmod +x ./docker-entrypoint.sh
RUN docker-php-ext-install pdo_mysql
CMD ["./docker-entrypoint.sh"]

WORKDIR /var/www/laravel