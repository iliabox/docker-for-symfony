FROM php:7.2-fpm-alpine
ARG TIMEZONE

RUN apk add --no-cache --virtual .persistent-deps \
  vim git unzip openssl \
  postgresql-dev \
  libxml2-dev \
  bash \
  autoconf g++ make

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
  && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install PDO extensions
RUN docker-php-ext-install pdo_pgsql

# Install redis
RUN pecl install -o -f redis \
  && rm -rf /tmp/pear \
  && docker-php-ext-enable redis

RUN docker-php-ext-install soap

RUN docker-php-ext-install bcmath

# Install pcntl
RUN docker-php-ext-install pcntl

COPY conf.d/* /usr/local/etc/php/conf.d/

# Create an alias for Symfony3 console
RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

#RUN adduser user

#CMD ["su", "-", "user", "-c", "/bin/bash"]

WORKDIR /var/www/symfony
