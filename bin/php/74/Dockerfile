FROM php:7.4-fpm

# Surpresses debconf complaints of trying to install apt packages interactively
# https://github.com/moby/moby/issues/4032#issuecomment-192327844

ARG DEBIAN_FRONTEND=noninteractive

# Update
RUN apt-get -y update --fix-missing && \
    apt-get upgrade -y && \
    apt-get --no-install-recommends install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*


# Install useful tools and install important libaries
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install \
    nano \
    wget \
    dialog \
    git \
    curl \
    libsqlite3-dev \
    zip \
    unzip \
    libssl-dev \
    libsqlite3-0 && \
    apt-get -y --no-install-recommends install \
    default-mysql-client \
    zlib1g-dev \
    libzip-dev \
    libicu-dev && \
    apt-get -y --no-install-recommends install --fix-missing \
    build-essential \
    libonig-dev && \
    apt-get -y --no-install-recommends install --fix-missing \
    libcurl4 \
    libcurl4-openssl-dev \
    libmcrypt-dev \
    openssl && \
    apt-get -y --no-install-recommends install --fix-missing \
    imagemagick \
    libfreetype6-dev \
    libmagickwand-dev \
    libjpeg-dev \
    libjpeg62-turbo-dev \
    libpng-dev && \
    rm -rf /var/lib/apt/lists/*


# Install xdebug
RUN pecl install xdebug-2.9.5 && \
    docker-php-ext-enable xdebug

# Install redis
RUN pecl install redis-5.2.2 && \
    docker-php-ext-enable redis

# Other PHP7 Extensions

RUN docker-php-ext-install zip && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install pdo_sqlite && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install curl && \
    docker-php-ext-install tokenizer && \
    docker-php-ext-install json && \
    docker-php-ext-install iconv && \
    docker-php-ext-install -j$(nproc) intl && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install gettext && \
    docker-php-ext-configure gd --with-jpeg=/usr/include/ --with-freetype=/usr/include/ && \
    docker-php-ext-install -j$(nproc) gd && \
    docker-php-ext-configure intl && \
    pecl install imagick-3.4.4 && \
    pecl install mcrypt && \
    pecl install xhprof && \
    docker-php-ext-enable imagick mcrypt xhprof opcache


RUN mkdir -p /var/log/php-fpm
RUN ln -sf /dev/stdout /var/log/php-fpm/access.log
RUN ln -sf /dev/stderr /var/log/php-fpm/error.log

ARG uid
ARG gid
ARG uname

RUN groupadd --gid $gid $uname
RUN useradd -G www-data,root -s /bin/bash --uid $uid --gid $gid $uname --home-dir /var/www

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && chown $uid:$gid /usr/local/bin/composer

RUN mkdir -p /var/www/.composer
RUN mkdir -p /var/www/.composer/cache
RUN chown -R $uid:$gid /var/www
RUN chown -R $uid:$gid /var/www/.composer

RUN mkdir /tmp/xhprof \
    && chown $uid:$gid /tmp/xhprof

#RUN mkdir -p /opt
#RUN cd /opt
#RUN wget -P /opt https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
#RUN tar vxf /opt/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz -C /opt
#RUN cp /opt/wkhtmltox/bin/wk* /usr/local/bin/
#RUN rm /opt/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz

ENV PHP_EXTRA_CONFIGURE_ARGS --enable-fpm --with-fpm-user=$uid --with-fpm-group=$gid

WORKDIR /var/www/html

CMD ["php-fpm"]

# Cleanup
RUN rm -rf /usr/src/*

USER $uname
