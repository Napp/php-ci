FROM ubuntu:20.04

MAINTAINER Mads Møller <mm@napp.dk>

ENV DEBIAN_FRONTEND=noninteractive

COPY build /build

RUN apt-get update \
    && apt-get upgrade -yq \
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
        php8.0 \
        php8.0-fpm \
        php8.0-bcmath \
        php8.0-bz2 \
        php8.0-exif \
        php8.0-imap \
        php8.0-intl \
        php8.0-ldap \
        php8.0-mbstring \
        php8.0-pdo \
        php8.0-pdo-mysql \
        php8.0-dom \
        php8.0-gd \
        php8.0-zip \
        php8.0-soap \
        php8.0-xdebug \
        php8.0-imagick \
        git \
        mysql-client \
        graphicsmagick \
        imagemagick \
        ghostscript \
        unzip \
        make \
    && /build/cleanup.sh

RUN apt-get update
RUN apt-get install -y php-pear php8.0-dev

RUN pecl install redis
RUN apt-get install -y php-redis \
    && /build/cleanup.sh

RUN mkdir -p /run/php && chown www-data:www-data /run/php

COPY container /

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

EXPOSE 9000
CMD ["php-fpm8.0"]
