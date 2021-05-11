FROM php:7.4-apache

RUN echo "memory_limit=-1" > "$PHP_INI_DIR/conf.d/memory-limit.ini" \
 && echo "date.timezone=${PHP_TIMEZONE:-UTC}" > "$PHP_INI_DIR/conf.d/date_timezone.ini"
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Install node and yarn
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash
RUN apt-get install -y nodejs
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg |  apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" |  tee /etc/apt/sources.list.d/yarn.list

RUN  apt-get update &&  apt-get install -y --no-install-recommends \
     locales apt-utils git libicu-dev g++ libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev unzip \
     libjpeg62-turbo-dev \
     libpng-dev \
     libfreetype6-dev \
     libjpeg-dev \
     libxpm-dev \
     libvpx-dev \
     libwebp-dev \
     libtool \
     yarn
RUN npm install jpegtran-bin
RUN docker-php-ext-install zip
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd
RUN docker-php-source delete

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen  \
    &&  echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen \
    &&  locale-gen

# Install composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer \
RUN chmod +x /usr/local/bin/composer
RUN adduser www-data root

# Install symfony cli
RUN curl -sS https://get.symfony.com/cli/installer | bash \
    &&  mv /root/.symfony/bin/symfony /usr/local/bin

RUN a2enmod rewrite
RUN pwd

RUN #!/bin/bash source /api/.env.local
RUN export $(cut -d= -f1 .env.local)

WORKDIR /var/www/
