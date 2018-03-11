#PHP 5.6 image
FROM php:5.6-apache
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        imagemagick \
        libmagickwand-dev \
        libmagickcore-dev \
        git \
        vim \
        mysql-client
#Install extensions
RUN pecl install xdebug-2.5.5
RUN docker-php-ext-enable xdebug
RUN docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-install -j$(nproc) pdo \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) opcache \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
#Copy php.ini with all the settings
#COPY config/php.ini /usr/local/etc/php/
#DRUSH
RUN curl -sS https://getcomposer.org/installer | php
#RUN ./composer.phar global require "drush/drush:7.x"
COPY ./config/php.ini /usr/local/etc/php/
EXPOSE 80
EXPOSE 9000


