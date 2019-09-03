FROM php:5.6-fpm

#----------------------------------------------------------
# Common
#----------------------------------------------------------

RUN apt-get update && apt-get install -y \
    apt-utils \
    libcurl4-openssl-dev \
    pkg-config \
    libssl-dev \
    zlib1g-dev \
    unzip \
    build-essential \
    gnupg \
    libz-dev \
    libmemcached-dev \
    libtool \
    automake \
    python-dev \
    libxml2-dev \
    git \
    libxslt-dev \
    automake python-dev \
    libmcrypt-dev \
    libpng-dev \
    procps \
    sudo

#----------------------------------------------------------
# PHP Libs
#----------------------------------------------------------

RUN docker-php-ext-install \
    pdo_mysql \
    bcmath \
    mysql \
    zip \
    soap \
    xsl \
    pcntl \
    mcrypt \
    gd \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.profiler_enable=0" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.profiler_enable_trigger=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.profiler_output_dir=/var/www/profiler" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo 'date.timezone = "Europe/Moscow"\n' > /usr/local/etc/php/conf.d/timezone.ini \
    && mkdir /var/www/profiler

#----------------------------------------------------------
# PHP Memcached
#----------------------------------------------------------

RUN pecl install igbinary-2.0.1 \
    && docker-php-ext-enable igbinary \
    && cd /var/www/html/ \
    && pecl download memcached-2.2.0 \
    && tar xzvf memcached-2.2.0.tgz \
    && cd memcached-2.2.0 \
    && phpize \
    && ./configure --enable-memcached-igbinary --disable-memcached-sasl \
    && make \
    && make install \
    && docker-php-ext-enable memcached \
    && rm -rf /var/www/html/*
