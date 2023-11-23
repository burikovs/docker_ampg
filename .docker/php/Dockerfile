FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
        ca-certificates cron curl git locate redis-tools software-properties-common wget zip\
        libmcrypt-dev \
        libzip-dev \
        libpq-dev \
        libwebp-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        libmemcached-dev \
        libmagickwand-dev  \
        procps \
	&& docker-php-ext-configure zip \
	&& docker-php-ext-install bcmath gd intl mysqli pdo_mysql pdo_pgsql soap sockets zip\
    && pecl install apcu imagick mcrypt memcached redis xdebug\
    && docker-php-ext-enable apcu imagick mcrypt memcached redis xdebug

RUN echo "deb https://download.keydb.dev/open-source-dist $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/keydb.list
RUN wget -O /etc/apt/trusted.gpg.d/keydb.gpg https://download.keydb.dev/open-source-dist/keyring.gpg
RUN apt-get update
RUN apt-get install -y keydb-tools

ADD ./crontab /etc/crontab
ADD ./conf/www.conf /usr/local/etc/php-fpm.d/www.conf

RUN chmod 0644 /etc/crontab

# install composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

WORKDIR /var/www/html

CMD ["php-fpm"]