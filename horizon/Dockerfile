# laravel.cron.Dockerfile
FROM php:8.1-fpm

WORKDIR /var

RUN apt-get update && apt-get install -y cron supervisor

# Configure supervisor
COPY supervisord.conf /etc/supervisor/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]

