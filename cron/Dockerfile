# laravel.cron.Dockerfile
FROM php:8.1-fpm

WORKDIR /var

# # Create the log file to be able to run tail
RUN touch /var/log/cron.log

RUN apt-get update && apt-get install -y cron supervisor

# Configure cron
RUN crontab -l | { cat; echo "* * * * * cd /var/www/html/rconfig6/current && /usr/local/bin/php artisan schedule:run >> /var/log/cron.log 2>&1"; } | crontab -

# Configure supervisor
COPY supervisord.conf /etc/supervisor/supervisord.conf

CMD  tail -f /var/log/cron.log  & /usr/bin/supervisord 
