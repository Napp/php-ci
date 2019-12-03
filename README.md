# Napp PHP GitLab CI Docker images

This repository contains a set of utilities for running PHP tests via [Gitlab CI](https://about.gitlab.com/gitlab-ci/).

These docker images can also be used as development images. 

# Supported tags and respective `Dockerfile` links

-	[`napp/php-ci:7.0-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.0/fpm/Dockerfile)
-	[`napp/php-ci:7.1-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.1/fpm/Dockerfile)
-	[`napp/php-ci:7.2-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.2/fpm/Dockerfile)
-	[`napp/php-ci:7.3-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.3/fpm/Dockerfile)
-	[`napp/php-ci:7.4-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.4/fpm/Dockerfile)

# Example

```yaml
stages:
  - test

before_script:
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

test_php74:
  image: napp/php-ci:7.4-fpm
  stage: test
  script:
    - phpunit --testsuite=unit

test_php73:
  image: napp/php-ci:7.3-fpm
  stage: test
  script:
    - phpunit --testsuite=unit

```

Or in a `docker-compose.yml` for development purposes. 

```yaml
version: '2'

services:
  php:
    image: napp/php-ci:7.4-fpm
    volumes:
      - '.:/var/www/html'
    # ...
```


## How to build 

Example of building one of the images

```
cd php/7.4/fpm
docker build -t napp/php-ci:7.4-fpm -f Dockerfile .
docker push napp/php-ci:7.4-fpm
```

