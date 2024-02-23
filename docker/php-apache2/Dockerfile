FROM php:8.2-apache

# Install dependencies
RUN apt-get clean && apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libicu-dev \
        zip libzip-dev unzip \
        libmagickwand-dev \
        libmcrypt-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql zip
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-configure intl && docker-php-ext-install intl
RUN pecl install imagick && docker-php-ext-enable imagick
RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN a2enmod rewrite

ARG user_id=1000
RUN usermod -u $user_id www-data

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/
