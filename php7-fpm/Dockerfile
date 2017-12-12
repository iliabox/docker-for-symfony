FROM php:7.2-fpm-alpine
ARG TIMEZONE

RUN apk add --no-cache \
  vim \
  bash

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
  && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apk add --no-cache \
  postgresql-dev \
  libxml2-dev \
  && docker-php-ext-install \
    pdo_pgsql \
    soap \
    bcmath \
    opcache \
    pcntl

RUN apk add --no-cache --virtual build-dependencies autoconf g++ make git \
  && docker-php-source extract \
  && pecl install \
    redis \
    apcu \
  && docker-php-ext-enable \
    redis \
    apcu \
  && docker-php-source delete \
  && mkdir /src && cd /src && git clone https://github.com/xdebug/xdebug.git && cd xdebug && sh ./rebuild.sh && cd / && rm -fr /src \
  && rm -rf /tmp/* \
  && apk del build-dependencies

COPY conf.d/* /usr/local/etc/php/conf.d/

USER www-data

RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/symfony
