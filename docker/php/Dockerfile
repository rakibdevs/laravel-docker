FROM php:8.1.4-fpm

RUN apt-get update \
    && apt-get -y install --no-install-recommends \
    locales \
    git \
    unzip \
    libzip-dev \
    libicu-dev \
    libonig-dev \
    && apt-get clean \
    && docker-php-ext-install \
    intl \
    pdo_mysql \
    zip \
    bcmath

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer
