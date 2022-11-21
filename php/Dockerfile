FROM php:8.1-apache

# Installing dependencies
RUN apt-get update && apt-get install -y vim wget curl default-mysql-client git

RUN apt-get install -y software-properties-common

RUN apt install -y g++ libicu-dev libpq-dev libzip-dev zip zlib1g-dev libcurl4-openssl-dev libonig-dev libgmp-dev libpng-dev libwebp-dev iputils-ping telnet ssh \
    && docker-php-ext-install intl opcache pdo pdo_mysql mysqli zip fileinfo bcmath gmp zip curl mbstring gd 
    
# Configure LDAP
RUN apt-get update \
    && apt-get install libldap2-dev -y \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install ldap

# Install pcntl
RUN docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-install pcntl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html/rconfig6/

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# copy composer 
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN chown -R www-data: /var/www/html/
RUN chmod -R 755 /var/www/html/
RUN a2enmod rewrite && service apache2 restart

RUN echo 'export COMPOSER_ALLOW_SUPERUSER=1' >>~/.bashrc
RUN echo 'export PATH=~/.config/composer/vendor/bin:$PATH' >>~/.bashrc
RUN echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >>~/.bashrc

# install laravel envoy
RUN composer global require laravel/envoy

# We must reconfigure here the CMD directive.
CMD apache2-foreground