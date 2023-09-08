FROM ubuntu:latest

ENV DEBIAN_FRONTEND noninteractive
ENV PHP_VERSION 8.2

RUN apt-get update
RUN apt-get install -y software-properties-common curl

RUN LC_ALL=C.UTF-8 apt-add-repository ppa:ondrej/php -y

RUN apt-get update && apt-get install -y \
    php${PHP_VERSION}-mcrypt \
    php${PHP_VERSION}-dom \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-cli \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-mysql \
    php${PHP_VERSION}-redis \
    php${PHP_VERSION}-fpm \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-decimal \
    supervisor \
    zip unzip \
    git \
    nano

RUN apt-get autoremove -y
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*

RUN mkdir -p /run/php && chmod -R 755 /run/php
RUN mkdir -p /var/www/html/log

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html/log
COPY . /var/www/html/log

ADD deploy/docker/php/custom.ini /etc/php/${PHP_VERSION}/fpm/conf.d/custom.ini
ADD deploy/docker/php/www.conf /etc/php/${PHP_VERSION}/fpm/pool.d/www.conf
ADD deploy/docker/supervisord/supervisord.conf /etc/supervisor/conf.d/supervisord.conf


RUN composer install --no-dev --no-progress

RUN chown -R www-data:www-data /var/www/html/log

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
