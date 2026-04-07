FROM php:8.4-fpm

# Системные зависимости
RUN apt-get update && apt-get install -y --no-install-recommends \
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
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# PHP расширения
RUN docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype \
    && docker-php-ext-install \
        pdo_mysql \
        mysqli \
        xml \
        mbstring \
        gd \
        zip \
        curl \
        gmp \
        bz2 \
        exif

# Установка pie
RUN curl -fsSL https://github.com/php/pie/releases/latest/download/pie.phar -o /usr/local/bin/pie \
    && chmod +x /usr/local/bin/pie

# PHP расширений через pie
RUN pie install imagick/imagick \
    && pie install php-memcached/php-memcached \
    && pie install xdebug/xdebug

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/enginegp

CMD ["php-fpm"]
