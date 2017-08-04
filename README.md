# Napp PHP CI Docker images

This repository contains a set of utilities for running PHP tests via [Gitlab CI](https://about.gitlab.com/gitlab-ci/).

# Supported tags and respective `Dockerfile` links

-	[`7.0`, (*7.0/Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.0/Dockerfile)
-	[`7.1`, (*7.1/Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.1/Dockerfile)

Images dont have `VOLUME` directories since fresh version of sources is supposed to be downloaded into image each time before running tests

These images are built from [Docker official php images](https://registry.hub.docker.com/_/php/), and additionally include:

 - All extensions are compiled and ready for loading with `docker-php-ext-enable`
 - PECL extensions: memcache, redis, mongo, xdebug
 - sendmail command via msmtp, configured as relay to localhost. Check `/etc/msmtprc` to setup relay server
 - Git client from official Debian repo
 - Composer
 - PHPUnit
 - PHP Code Sniffer
 - PHP Mess Detector
 - Node.js v7 from official Node.js Debian repositories
 - mysql client for database manipulations

See below for details

## PHP modules
Some modules are enabled by default (compiled-in) and some you have to enable in your .gitlab-ci.yml `before_script` section with `docker-php-ext-enable module1 module2`

### Compiled-in modules
```
ctype curl date dom ereg fileinfo filter hash iconv json libxml mysqlnd openssl pcre pdo pdo_sqlite phar posix readline recode reflection session simplexml spl sqlite3 standard tokenizer xml xmlreader xmlwriter zlib
```

### Available core modules
```
bcmath bz2 calendar dba enchant exif ftp gd gettext gmp imap intl ldap mbstring mcrypt mssql mysql mysqli opcache pcntl pdo pdo_dblib pdo_mysql pdo_pgsql pgsql pspell shmop snmp soap sockets sysvmsg sysvsem sysvshm tidy wddx xmlrpc xsl zip
```

### Available PECL modules
```
memcache memcached mongo mongodb redis xdebug
```

## Environment variables

There are environment variables which can be passed to images on docker run

 - `WITH_XDEBUG=1` - enables xdebug extension
 - `TIMEZONE=America/New_York` - set system and `php.ini` timezone. You can also set timezone in .gitlab-ci.yml
 - `COMPOSER_GITHUB=<YOUR_GITHUB_TOKEN>` - Adds Github oauth token for composer which allows composer to get unlimited repositories from Github without blocking non-interactive mode with request for authorization. You can obtain your token at [https://github.com/settings/tokens](https://github.com/settings/tokens)

    [Composer documentation about Github API rate limit](https://getcomposer.org/doc/articles/troubleshooting.md#api-rate-limit-and-oauth-tokens)

# FAQ

1. **How to set custom php.ini values**

   Easiest way is to add your php.ini directives to `/usr/local/etc/php/conf.d/[anyname].ini`
   Another way is to mount your local php.ini on container start like `docker run ... -v /home/user/php.ini:/usr/local/php/etc/php.ini ...`


# Example

```yaml
stages:
  - test

before_script:
  # Enable PHP Extensions
  - docker-php-ext-enable zip soap gd pdo_mysql mysqli

  # Composer
  - composer self-update
  - composer install --no-progress --no-interaction

variables:
  WITH_XDEBUG: "1"
  MYSQL_ROOT_PASSWORD: mysql
  MYSQL_DATABASE: mydb
  MYSQL_USER: myuser
  MYSQL_PASSWORD: somepassword
  COMPOSER_HOME: /cache/composer
  REDIS_PORT: "6379"

test_php70:
  image: napp/php:7.0
  stage: test
  script:
    - phpunit --testsuite=unit

test_php71:
  image: napp/php:7.1
  stage: test
  script:
    - phpunit --testsuite=unit

```

