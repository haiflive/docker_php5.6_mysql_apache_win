FROM php:5.6.32-apache

# Keep everything up-to-date
RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y

# Install useful tools
RUN apt-get -y install apt-utils nano wget dialog locate

## Install Mysql Support
RUN apt-get -y install libsqlite3-dev libsqlite3-0 mysql-client
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_sqlite
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install mysql

## Install libfreetype, mcrypt, gd with freetype support
RUN apt-get update && apt-get install -y \
		curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

## Install Redis and Xdebug
RUN pecl install redis-3.1.0 \
    && pecl install xdebug-2.5.0 \
    && docker-php-ext-enable redis xdebug

## Install Memcached
RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev libicu-dev g++ \
    && pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached \
	&& docker-php-ext-configure intl \
	&& docker-php-ext-install intl
	
## Enable SSL
RUN a2enmod ssl

# Enable Mod Rewrite
RUN a2enmod rewrite

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

# User mod the users so they run under the correct permissions.
RUN usermod -u 1000 www-data

WORKDIR /var/www
