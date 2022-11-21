# laravel.cron.Dockerfile
FROM php:8.1-fpm

WORKDIR /var

# Installing dependencies
RUN apt-get update && apt-get install -y vim wget curl default-mysql-client git supervisor

RUN apt-get install -y software-properties-common

RUN apt install -y g++ libicu-dev libpq-dev libzip-dev zip zlib1g-dev libcurl4-openssl-dev libonig-dev libgmp-dev libpng-dev libwebp-dev iputils-ping telnet ssh \
    libjpeg62-turbo-dev libfreetype6-dev locales \
    && docker-php-ext-install intl opcache pdo pdo_mysql mysqli zip fileinfo bcmath gmp zip curl mbstring gd 

# Install pcntl
RUN docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-install pcntl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Configure supervisor
COPY supervisord.conf /etc/supervisor/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]

