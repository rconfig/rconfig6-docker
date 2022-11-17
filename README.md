Notes:
Supported OS -
Rocky Linux 8.4+
RHEL Linux 8.4+
CentOS Linux 8.4+

Not tested on Docker Desktop or Docker for Windows

1. create composer and dockerfiles
2. create top level .env file
3. login to `docker-compose exec php-apache /bin/bash`
4. download envoy script
   `wget https://www.rconfig.com/downloads/rconfig6-docker-envoy.blade.php -O /var/www/html/rconfig6/Envoy.blade.php`

5. run the envoy init
   `envoy run init --dbhost=$DB_HOST --dbuser=$DB_USERNAME --dbpass=$DB_PASSWORD --hostname=$APP_URL --apitoken=$RCONFIG_API_TOKEN`
6. run the envoy deploy
   `envoy run deploy --apitoken=$RCONFIG_API_TOKEN`

<!-- References:
https://www.twilio.com/blog/get-started-docker-laravel
https://laravel-for-newbie.kejyun.com/en/advanced/scheduling/docker/
https://github.com/mohammadain/laravel-docker-cron/blob/master/Dockerfile -->
