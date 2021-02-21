FROM php:7.4.3-fpm-alpine

RUN apk add --no-cache libzip-dev bash

WORKDIR /var/www/html

COPY index.php /var/www/html/index.php
COPY docker-php-entrypoint /usr/local/bin/docker-php-entrypoint

RUN chmod +x /usr/local/bin/docker-php-entrypoint

ENTRYPOINT ["docker-php-entrypoint"]
CMD [ "php-fpm", "--allow-to-run-as-root", "--nodaemonize" ]
