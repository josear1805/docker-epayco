FROM php:8.1-apache

ENV TZ=America/Guayaquil
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y unzip nano \
    && apt-get autoremove -y \
    && rm -r /var/lib/apt/lists/*

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/
RUN install-php-extensions gd pdo_mysql soap zip redis imap intl memcached mongodb gmp

COPY files/000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY files/app.conf /etc/apache2/conf-enabled/z-app.conf
COPY files/app.ini $PHP_INI_DIR/conf.d/app.ini

RUN a2enmod headers rewrite deflate

COPY --from=composer:2.4.4 /usr/bin/composer /usr/bin/composer

# commando
# docker build -t epayco:php-8.1 .