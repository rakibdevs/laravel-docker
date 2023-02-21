FROM php:8.1.4-fpm

RUN apt-get update \
    && apt-get install -y \
    libpq-dev \
    libonig-dev \
    locales \
    libzip-dev \
    unzip 

RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    mbstring \
    pcntl \
    bcmath \
    zip \
    gd

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer
