FROM php:7.1-fpm
ARG TIMEZONE

RUN apt-get update && apt-get install -y vim git unzip openssl

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install PDO extensions
RUN apt-get install -y libpq-dev \
  && docker-php-ext-install pdo_pgsql

# Install redis
RUN pecl install -o -f redis \
  &&  rm -rf /tmp/pear \
  &&  docker-php-ext-enable redis

# Install pcntl
#RUN docker-php-ext-install pcntl

RUN apt-get install libxml2-dev \
  && docker-php-ext-install soap

RUN docker-php-ext-install bcmath

# Create an alias for Symfony3 console
RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

#RUN adduser user

# Install xdebug
#RUN pecl install xdebug && docker-php-ext-enable xdebug
#COPY conf.d/* /usr/local/etc/php/conf.d/

#CMD ["su", "-", "user", "-c", "/bin/bash"]

WORKDIR /var/www/symfony
