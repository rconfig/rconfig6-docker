<!-- References:
https://www.twilio.com/blog/get-started-docker-laravel
https://laravel-for-newbie.kejyun.com/en/advanced/scheduling/docker/
https://github.com/mohammadain/laravel-docker-cron/blob/master/Dockerfile -->

<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->

<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/rconfig/rconfig6-docker">
    <img src="https://www.rconfig.com/images/new_logos/blue_logos/artwork_blue_horizontal_Artboard_1_96px.png" alt="Logo" >
  </a>

  <h3 align="center">rConfig v6 Docker Compose Repository</h3>

  <p align="center">
    A repo for to setup containers for the purpose of running rConfig in Docker.
    <br />
    <a href="https://www.rconfig.com/docs"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/rconfig/rconfig6-docker/#intro">Intro</a>
    ·
    <a href="https://github.com/rconfig/rconfig6-docker/#setup">Installation</a>
    ·
    <a href="https://github.com/rconfig/rconfig6-docker/#usage">Usage</a>
    ·
    <a href="https://github.com/rconfig/rconfig6-docker/#contributing">Contributing</a>
    ·
    <a href="https://github.com/rconfig/rconfig6-docker/#license">License</a>
    ·
    <a href="https://github.com/rconfig/rconfig6-docker/#support">Support</a>
  </p>
</div>

<!-- Intro -->

<a name="intro"></a>

## Intro

rConfig v6 is an enterprise grade Network Configuration Management (NCM) software package with superior NCM features and capabilities to help you easily manage configurations on large and small heterogenous networks. rConfig v6 is our flagship professional version of rConfig aimed at high value networks and business operations. rConfig v6 runs natively on many variants of Linux. Within this repo, we have developed docker compose files and related artifacts to allow our customers run rConfig v6 within a Docker environment.

Supported OS

- Rocky Linux 8.4+
- RHEL Linux 8.4+
- CentOS Linux 8.4+

These scripts are not tested on Docker Desktop or Docker for Windows at the time of this commit.

Of course, you may download or clone these files, and edit as you see fit. But you will need an rConfig v6 professional license to download rConfig v6 code base. This is available over at rconfig.com.

Check out our docs `www.rconfig.com/docs` to learn more.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Installation -->

<a name="setup"></a>

## Installation

We have made it super easy to get started with rConfig v6 on Docker. Follow the steps below to get started.

### Prerequisites

Some prerequisites are needed before you get started.

- OS
  - Any latest version of CentOS, RHEL, Rocky or Ubunutu
- Docker
  - Any latest version of Docker or Docker-CE
  - Any latest version of Docker Compose

Your OS will need `git` installed to clone this repo. Internet access for the host VM is assumed also - at least to github and rconfig.com.

#### Useful URLS

- [Docker installation for CentOS/ Rocky](https://docs.docker.com/engine/install/centos/)
- [Manual Docker Compose installation for CentOS/ Rocky](https://docs.docker.com/compose/install/other/) (useful incase previous does not install it )

### Installation

1. Clone this repository to a directory of your choosing.

   ```sh
   cd /var/www/html
   git clone https://github.com/rconfig/rconfig6-docker.git
   ```

2. Create top level .env file

   ```sh
   cd rconfig6-docker
   cp .env.example .env
   ```

3. Edit the .env file with your DB and other parameters

   ```sh
   vim .env
   ```

   ```sh
   APP_URL=somehostname.yourdomain.com
   APP_PORT=8080

   DB_CONNECTION=mysql
   DB_HOST=rconfig6-mariadb
   DB_PORT=3306
   DB_USERNAME=root
   DB_PASSWORD=

   RCONFIG_API_TOKEN=

   #REDIS ENV CONFIG
   REDIS_HOST=rconfig6-redis
   REDIS_PASSWORD=null
   REDIS_PORT=6379

   #EXPOSED PORTS
   EXPOSED_APP_PORT=8080
   ```

If you are uncertain about any of the above, the most important items to change are the DB_PASSWORD and the RCONFIG_API_TOKEN

4. Bring up the containers. This time we remove the Horizon container from the build as the application init is not complete so will throw errors. (This will take a few minutes the first time)

   ```sh
   docker-compose up --scale horizon=0 -d --build
   ```

5. Once the comtainers are up and running with no errors from the previous output,
   login to the php-apache container

   ```sh
   docker-compose exec php-apache /bin/bash
   ```

6. Download envoy script

   ```sh
   wget https://www.rconfig.com/downloads/rconfig6-docker-envoy.blade.php -O /var/www/html/rconfig6/Envoy.blade.php
   ```

7. Run the envoy init

   ```sh
   envoy run init --dbhost=$DB_HOST --dbuser=$DB_USERNAME --dbpass=$DB_PASSWORD --hostname=$APP_URL --apitoken=$RCONFIG_API_TOKEN
   ```

8. run the envoy deploy

   ```sh
   envoy run deploy --apitoken=$RCONFIG_API_TOKEN
   ```

9. Once the installation completes with no errors, exit the container, bring everything down, and start all containers up.

   ```sh
   exit
   docker-compose down
   docker-compose up -d --build
   ```

10. your final output from the previous step should look like this

```sh
[+] Running 6/6
⠿ Network rconfig6-docker_rconfig6Network  Created       0.3s
⠿ Container rconfig6-redis                 Started       0.6s
⠿ Container rconfig6-mariadb               Started       0.7s
⠿ Container rconfig6-php-apache            Started       1.1s
⠿ Container rconfig6-horizon-supervisor    Started       1.6s
⠿ Container rconfig6-cronjob               Started       1.6s
```

When troubleshooting issues, the following commands are useful.
`docker ps -a`
`docker logs CONTAINERNAME`

#### Disk or storage permissions issues

If you have permissions issues, such as errors that rConfig cannot write to the storage directory, follow these steps.

1. Login to the php-apache container

   ```sh
   docker-compose exec php-apache /bin/bash
   ```

2. View the users list and find the id for the `www-data` account

   ```sh
   more /etc/passwd | grep www
   ```

3. The output should be similar to this

   ```sh
   www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
   ```

4. Simply apply the correct permissions to the `persistentdata` directory
   ```sh
   chown -R 33 persistentdata/
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->

<a name="usage"></a>

## Usage

The default port for the web app is 8080. So go ahead and login to yourhostname.domain.com:8080 with the rConfig default creds per the documentation.

_Please refer to the [Documentation](https://www.rconfig.com/docs)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->

<a name="contributing"></a>

## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->

<a name="license"></a>

## License

This code base for this repository's code is distributed under the MIT License. See `LICENSE.txt` for more information. rConfig v6 is excluded from this license and repository.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- https://github.com/othneildrew/Best-README-Template/blob/master/README.md -->

<a name="support"></a>

## Support

Although we provide this code free and open source, rConfig v6 is based on a professional subscription arrangement. You may open issues in the issue section here at github, but rConfig Professional subscribers should open a ticket via our normal support channels.
