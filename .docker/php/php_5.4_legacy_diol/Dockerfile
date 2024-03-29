FROM php:5.4.45-fpm

RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y  \
  git \
  mercurial \
  zip \
  nano \
  php5\
  php5-fpm \
  php5-cli \
  php5-readline \
  php5-dev libmcrypt-dev php-pear \
  php5-mysql \
  php5-mcrypt \
  php5-curl \
  php5-gd \
  php5-intl \
  libpng-dev \
  libbz2-dev \
  libxml2-dev \
  libmcrypt-dev \
  php5-xdebug \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  mysql-client

# Clear cache
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  /tmp/* \
  /var/tmp/*

RUN php5enmod mcrypt

RUN docker-php-ext-install pdo pdo_mysql pcntl \
    mcrypt \
    mbstring \
    zip \
    exif \
    bcmath \
    bz2 \
    calendar \
    dba \
    ftp \
    hash \
    shmop \
    soap \
#    SimpleXML \
    sysvmsg \
    sysvsem \
    sysvshm \
    wddx \
    gd \
    mysql \
    mysqli \
    mbstring

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN yes | pecl install xdebug-2.4.1 \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini

RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_log=/tmp/xdebug.log" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.profiler_enable_trigger=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.profiler_output_dir=/var/www/.docker/xdebug_profiler/" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.var_display_max_depth = -1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.var_display_max_children = -1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#    && echo "xdebug.max_nesting_level = 15" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.var_display_max_data = -1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer

ENV COMPOSER_ALLOW_SUPERUSER=1
RUN composer selfupdate --1

ADD ./.docker/php/php_5.4_legacy_diol/php.ini  /usr/local/etc/php/php.ini

ENV USER=denisl
ENV UID=1000
ENV GID=1000

RUN groupadd -g "$GID" "$USER"
RUN useradd -u "$UID" -ms /bin/bash -g "$USER" "$USER"

USER "$USER"

WORKDIR /var/www
