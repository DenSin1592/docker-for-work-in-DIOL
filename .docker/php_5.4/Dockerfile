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
  mysql-client

RUN docker-php-ext-install pdo pdo_mysql pcntl
# Clear cache
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  /tmp/* \
  /var/tmp/*

# Configure nginx
RUN mkdir -p /var/www

# Configure PHP
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/fpm/php.ini
RUN sed -i "s/;date.timezone =.*/date.timezone = europe\/moscow/" /etc/php5/fpm/php.ini
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php5/fpm/php-fpm.conf
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/cli/php.ini
RUN sed -i "s/;date.timezone =.*/date.timezone = europe\/moscow/" /etc/php5/cli/php.ini
RUN php5enmod mcrypt

RUN apt-get update && \
    apt-get install -y libmcrypt-dev

RUN docker-php-ext-install mcrypt

RUN docker-php-ext-install mbstring
RUN docker-php-ext-install zip
RUN docker-php-ext-install exif

RUN docker-php-ext-install gd
RUN docker-php-ext-configure gd

RUN apt-get update && \
            apt-get install -y libfreetype6-dev libjpeg62-turbo-dev && \
            docker-php-ext-install mysqli && \
            docker-php-ext-install mbstring && \
            docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/  &&  \
            docker-php-ext-install gd

RUN apt-get install php5-xdebug

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
    && echo "xdebug.var_display_max_data = -1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN chmod 777 -R /var/www/

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer

ENV COMPOSER_ALLOW_SUPERUSER=1

ADD php.ini /usr/local/etc/php/php.ini

WORKDIR /var/www