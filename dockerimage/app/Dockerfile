FROM php:7.2.6-apache

RUN apt-get update

# install zip
RUN apt-get install -y libzip-dev unzip zip
RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip


COPY php.ini /usr/local/etc/php/

# Install PDO and mysql PGSQL Drivers
RUN apt-get install -y libpq-dev
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-install pdo pdo_mysql pdo_pgsql pgsql mysqli mbstring

# Install Composer
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd bcmath intl opcache
COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer

# install mcrypt
RUN apt-get update -y && \
    apt-get install -y libmcrypt-dev && \
    pecl install mcrypt-1.0.1 && \
    docker-php-ext-enable mcrypt

RUN chmod 777 -R /var/www/html
WORKDIR /var/www/html

RUN a2enmod rewrite