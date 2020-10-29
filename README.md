# Napp PHP Docker images


# Supported tags and respective `Dockerfile` links

-	[`napp/php-ci:7.0-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.0/fpm/Dockerfile)
-	[`napp/php-ci:7.1-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.1/fpm/Dockerfile)
-	[`napp/php-ci:7.2-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.2/fpm/Dockerfile)
-	[`napp/php-ci:7.3-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.3/fpm/Dockerfile)
-	[`napp/php-ci:7.4-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/7.4/fpm/Dockerfile)
-	[`napp/php-ci:8.0-fpm`, (*Dockerfile*)](https://github.com/Napp/php-ci/blob/master/php/8.0/fpm/Dockerfile)

- Added `php-ci:8.0-rc1-fpm`

# Example

## Docker Compose 

in a `docker-compose.yml` 

```yaml
version: '3'

services:
  php:
    image: napp/php-ci:8.0-rc2-fpm
    volumes:
      - '.:/var/www/html'
    # ...
```


## GitLab CI

Example use in GitLab CI

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

test_php:
  image: napp/php-ci:8.0-rc2-fpm
  stage: test
  script:
    - phpunit --testsuite=unit

test_php73:
  image: napp/php-ci:7.4-fpm
  stage: test
  script:
    - phpunit --testsuite=unit

```




## How to build 

Example of building one of the images

```
cd php/8.0/fpm
docker build --no-cache -t napp/php-ci:8.0-fpm -f Dockerfile .
docker push napp/php-ci:8.0-fpm
```

