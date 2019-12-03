FROM ubuntu:18.04

MAINTAINER Mads Møller <mm@napp.dk>

ENV DEBIAN_FRONTEND=noninteractive

COPY build /build

RUN apt-get update \
    && apt-get install -yq --no-install-recommends \
        apt-utils \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common \
    && LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php \
    && apt-get remove --purge -y software-properties-common \
    && /build/cleanup.sh

RUN apt-get update \
    && apt-get install -yq --no-install-recommends \
        php7.4 \
        php7.4-fpm \
        php7.4-bcmath \
        php7.4-bz2 \
        php7.4-curl \
        php7.4-exif \
        php7.4-iconv \
        php7.4-imap \
        php7.4-intl \
        php7.4-ldap \
        php7.4-mbstring \
        php7.4-pdo \
        php7.4-pdo-mysql \
        php7.4-pdo-pgsql \
        php7.4-sqlite3 \
        php7.4-json \
        php7.4-phar \
        php7.4-dom \
        php7.4-gd \
        php7.4-ctype \
        php7.4-zip \
        php7.4-soap \
        php7.4-xdebug \
        php7.4-imagick \
        git \
        mysql-client \
        graphicsmagick \
        imagemagick \
        ghostscript \
        unzip \
        make \
    && /build/cleanup.sh

RUN apt-get update
RUN apt-get install -y php-pear php7.4-dev

RUN pecl install redis
RUN apt-get install -y php-redis

RUN mkdir -p /run/php && chown www-data:www-data /run/php

COPY container /

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

EXPOSE 9000
CMD ["php-fpm7.4"]
