FROM php:7.0-fpm

MAINTAINER Carw "carw.master@gmail.com"

ADD ./pool.conf /usr/local/etc/php-fpm.d/

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
