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
        php7.2 \
        php7.2-fpm \
        php7.2-bcmath \
        php7.2-bz2 \
        php7.2-curl \
        php7.2-exif \
        php7.2-iconv \
        php7.2-imap \
        php7.2-intl \
        php7.2-ldap \
        php7.2-mbstring \
        php7.2-pdo \
        php7.2-pdo-mysql \
        php7.2-pdo-pgsql \
        php7.2-sqlite3 \
        php7.2-json \
        php7.2-phar \
        php7.2-dom \
        php7.2-gd \
        php7.2-ctype \
        php7.2-zip \
        php7.2-soap \
        php7.2-xdebug \
        php7.2-imagick \
        git \
        mysql-client \
        graphicsmagick \
        imagemagick \
        ghostscript \
        unzip \
        make \
    && /build/cleanup.sh

RUN apt-get update
RUN apt-get install -y php-pear php7.2-dev

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
CMD ["php-fpm7.2"]