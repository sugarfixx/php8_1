FROM php:8.1-fpm-alpine3.15

COPY wait-for-it.sh /usr/bin/wait-for-it

RUN chmod +x /usr/bin/wait-for-it

# Install packages
RUN apk --no-cache add php8-pdo php8-pgsql php8-tokenizer php8-xmlwriter php8-mongodb php8-pdo_pgsql php8-intl php8-gd  \
    nginx supervisor curl curl-dev postgresql-dev libressl-dev pkgconfig zip vim php8-pear php8-dev gcc musl-dev make

RUN docker-php-ext-install pdo pdo_pgsql

ENV LIBRDKAFKA_VERSION v1.6.2
ENV BUILD_DEPS \
  autoconf \
        bash \
        build-base \
        git \
        pcre-dev \
        python3 \
        libressl-dev
RUN apk --no-cache --virtual .build-deps add ${BUILD_DEPS} \
                              && cd /tmp \
                              && git clone \
                                  --branch ${LIBRDKAFKA_VERSION} \
                                  --depth 1 \
                                  https://github.com/edenhill/librdkafka.git \
                              && cd librdkafka \
                              && ./configure \
                              && make \
                              && make install \
                              && pecl install rdkafka \
                              && rm -rf /tmp/librdkafka \
                              && apk del .build-deps



COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

CMD composer i -o; php-fpm  

EXPOSE 9000
