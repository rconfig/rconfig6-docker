# laravel.cron.Dockerfile
FROM php:8.1-fpm

WORKDIR /var

# Installing dependencies
RUN apt-get update && apt-get install -y \
    supervisor \
    default-mysql-client \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev zlib1g-dev libcurl4-openssl-dev libonig-dev libgmp-dev libpng-dev libwebp-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Installing extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl

# Configure supervisor
COPY supervisord.conf /etc/supervisor/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]

