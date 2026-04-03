FROM php:8.4-fpm

# Системные зависимости
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    libfreetype6-dev \
    libmagickwand-dev \
    libzip-dev \
    libgmp-dev \
    libbz2-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    libonig-dev \
    libmemcached-dev \
    default-mysql-client \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# PHP расширения
RUN docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype \
    && docker-php-ext-install \
        mysqli \
        xml \
        mbstring \
        gd \
        zip \
        curl \
        gmp \
        bz2 \
        exif

# imagick через PECL
RUN pecl install imagick \
    && docker-php-ext-enable imagick

# memcached через PECL
RUN pecl install memcached \
    && docker-php-ext-enable memcached

# Xdebug для разработки
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/enginegp

CMD ["php-fpm"]
