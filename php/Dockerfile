FROM php:7.2-fpm

LABEL maintainer="ninja911<ninja911@qq.com>"

ENV TIMEZONE Asia/Shanghai
ENV SWOOLE_VERSION 4.0.3
ENV PHALCON_VERSION 3.4.1

# replace sources.list
RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone

# init
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    wget \
    zip \
    unzip \
    git \
    unzip \
    procps \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libmemcached-dev \
    apt-utils \
    && rm -rf /var/lib/apt/lists/*

# install hiredis
RUN wget --no-check-certificate https://gitee.com/oschina/hiredis/repository/archive/v0.13.3.zip -O hiredis.zip \
    && mkdir -p hiredis \
    && unzip hiredis.zip \
    && rm hiredis.zip \
    && ( \
        cd hiredis \
        && make -j$(nproc) \
        && make install \
        && ldconfig \
    ) \
    && rm -r hiredis

# install php extentions
RUN pecl channel-update pecl.php.net \
    && docker-php-ext-configure gd \
    --enable-gd \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-install -j$(nproc) bcmath mysqli pdo_mysql gd \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install inotify \
    && docker-php-ext-enable inotify \
    && pecl clear-cache

# install phalcon extention
RUN wget --no-check-certificate https://gitee.com/mirrors/cphalcon/repository/archive/v${PHALCON_VERSION}.zip -O phalcon.zip \
    && unzip phalcon.zip \
    && rm phalcon.zip \
    && ( \
        cd cphalcon/build \
        && ./install \
    ) \
    && rm -r cphalcon \
    && docker-php-ext-enable phalcon


# install swoole extention
RUN wget --no-check-certificate https://gitee.com/swoole/swoole/repository/archive/v${SWOOLE_VERSION}.zip -O swoole.zip \
    && mkdir -p swoole \
    && unzip swoole.zip \
    && rm swoole.zip \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-async-redis --enable-mysqlnd --enable-openssl \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole


# install composer
RUN curl -sS https://install.phpcomposer.com/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repositories.packagist composer https://packagist.phpcomposer.com \
    && composer self-update --clean-backups \
    && composer config -g repo.packagist composer https://packagist.laravel-china.org

EXPOSE 9000
CMD ["php-fpm"]