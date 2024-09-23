FROM php:7.4.13-fpm-alpine3.12

LABEL maintainer="ahbanavi@gmail.com"
LABEL description="This image is 7.4.13-fpm-alpine3.12 with pdo_mysql and redis extension installed"
LABEL version="1.0"

# Install redis extension and pdo_mysql
RUN apk --no-cache add pcre-dev ${PHPIZE_DEPS} \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install pdo_mysql \
    && apk del pcre-dev ${PHPIZE_DEPS} \
    && rm -rf /tmp/pear