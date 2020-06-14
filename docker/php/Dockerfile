FROM php:7.4-fpm-buster

ARG LARAVEL_ROOT=/usr/share/nginx/html

RUN apt-get update

# gd
RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

# zip
RUN apt-get install -y \
        libzip-dev \
    && docker-php-ext-install zip


# BCMath
RUN docker-php-ext-install -j$(nproc) pdo pdo_mysql bcmath
RUN docker-php-ext-enable bcmath

# Zip
RUN apt-get install -y zlib1g-dev
RUN docker-php-ext-install -j$(nproc) zip
RUN docker-php-ext-enable zip

# Exif
RUN docker-php-ext-configure exif
RUN docker-php-ext-install exif
RUN docker-php-ext-enable exif

# Imagick
RUN apt-get install -y libmagickwand-dev
RUN yes | pecl install imagick && echo "extension=$(find /usr/local/lib/php/extensions/ -name imagick.so)" > /usr/local/etc/php/conf.d/imagick.ini

# FPM
RUN echo 'php_flag[display_errors] = on' >> /usr/local/etc/php-fpm.d/www.conf
RUN echo 'php_admin_flag[log_errors] = on' >> /usr/local/etc/php-fpm.d/www.conf

# Redis
RUN pecl install redis && docker-php-ext-enable redis


# XDebug
RUN yes | pecl install xdebug && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini

# Opcache.
# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=2'; \
    echo 'opcache.fast_shutdown=1'; \
    echo 'opcache.enable_cli=1'; \
} > /usr/local/etc/php/conf.d/opcache-recommended.ini


# Install composer
RUN apt-get install -y unzip
ADD https://composer.github.io/installer.sig .
ADD https://getcomposer.org/installer composer-setup.php
RUN echo -n $(shasum -a 384 ./composer-setup.php | cut -d " " -f 1) >> calculated
RUN diff installer.sig calculated
RUN php composer-setup.php --install-dir /usr/local/bin --filename composer
RUN rm installer.sig composer-setup.php calculated

WORKDIR $LARAVEL_ROOT
