# docker build -t docker.hw.apm.group/php:pg .
# $PHP_INI_DIR = /usr/local/etc/php

FROM php:5.6-apache

RUN sed -i -e 's/deb.debian.org/archive.debian.org/g' \
           -e 's|security.debian.org|archive.debian.org/|g' \
           -e '/stretch-updates/d' /etc/apt/sources.list

RUN apt update && apt install -y \
    ca-certificates \
    libpq-dev \
    libgd-dev \
    libgmp-dev \
    libmagick++-dev \
    libmcrypt-dev \
    libssh2-1-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h

RUN apt upgrade -y openssl

RUN update-ca-certificates

RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    calendar \
    exif \
    gd \
    gettext \
    gmp \
    intl \
    mcrypt \
    mysqli \
    opcache \
    pcntl \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    shmop \
    soap \
    sockets \
    xmlrpc \
    zip

RUN pecl channel-update pecl.php.net

RUN yes '' | pecl install -f imagick
RUN docker-php-ext-enable imagick

#RUN yes '' | pecl install -f ssh2
#RUN docker-php-ext-enable ssh2

RUN yes '' | pecl install -f rar
RUN docker-php-ext-enable rar

RUN apt autoremove -y && apt clean autoclean && rm -rf /var/lib/{apt,dpkg/info,cache,log}

# get real ip
RUN sed -i 's/%h/%{X-Forwarded-For}i/g' /etc/apache2/apache2.conf

# enable apache modules
RUN a2enmod rewrite

#RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
#RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
