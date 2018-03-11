#PHP 7.1 image
FROM php:7.1-apache
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
        mysql-client \
        zip
#Install extensions
RUN pecl install xdebug
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
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-enable imagick \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
#Copy php.ini with all the settings
ADD ./config/php.ini /usr/local/etc/php/php.ini
#Install oci8 extension
# Oracle instantclient
RUN apt-get update && apt-get install -y libaio1

ADD config/instantclient-basic-linux.x64-12.2.0.1.0.zip /tmp/
ADD config/instantclient-sdk-linux.x64-12.2.0.1.0.zip /tmp/
ADD config/instantclient-sqlplus-linux.x64-12.2.0.1.0.zip /tmp/

RUN unzip /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip -d /usr/local/
RUN unzip /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip -d /usr/local/
RUN unzip /tmp/instantclient-sqlplus-linux.x64-12.2.0.1.0.zip -d /usr/local/
RUN ln -s /usr/local/instantclient_12_2 /usr/local/instantclient
RUN ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so
RUN ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus
RUN echo /usr/local/instantclient > /etc/ld.so.conf.d/oracle-instantclient
RUN ldconfig
RUN echo 'instantclient,/usr/local/instantclient' | pecl install oci8
RUN echo "extension=oci8.so" > /usr/local/etc/php/conf.d/docker-oci8.ini

EXPOSE 80
EXPOSE 9000


