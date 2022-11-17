<!-- References:
https://www.twilio.com/blog/get-started-docker-laravel
https://laravel-for-newbie.kejyun.com/en/advanced/scheduling/docker/
https://github.com/mohammadain/laravel-docker-cron/blob/master/Dockerfile -->

<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->

<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/rconfig/-rconfig6-docker">
    <img src="https://www.rconfig.com/images/new_logos/blue_logos/artwork_blue_horizontal_Artboard_1_96px.png" alt="Logo" >
  </a>

  <h3 align="center">rConfig v6 Docker Compose Repository</h3>

  <p align="center">
    A repo for to setup containers for the purpose of running rConfig in Docker.
    <br />
    <a href="https://www.rconfig.com/docs"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/rconfig/-rconfig6-docker/#intro">Intro</a>
    ·
    <a href="https://github.com/rconfig/-rconfig6-docker/#setup">Installation</a>
    ·
    <a href="https://github.com/rconfig/-rconfig6-docker/#support">Support</a>
  </p>
</div>

<!-- Intro -->

<a name="intro"></a>

## Intro

rConfig v6 is an enterprise grade Network Configuration Management (NCM) software package with superior NCM features and capabilities to help you easily manage configurations on large and small heterogenous networks. rConfig v6 is our flagship professional version of rCconfig aimed at high value networks and business operations. rConfig v6 runs nativley on many variants of Linux. Within this repo, we have developed docker compose files and related artifacts to allow our customers run rConfig v6 within a Docker environment.

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
  - Any latest version of CentOS, RHEL, Rocky or Ubunut
- Docker
  - Any latest version of Docker or Docker-CE

Your OS will need `git` installed to clone this repo. Internet access for the host VM is assumed also - at least to github and rconfig.com.

### Installation

1. Clone this repository to a directory of your choosing.

   ```sh
   cd /var/www/html
   git clone https://github.com/rconfig/-rconfig6-docker.git
   ```

2. Create top level .env file

   ```sh
   cp .env.example .env
   ```

3. Edit the .env file with your DB and other parameters

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
   EXPOSED_DB_PORT=3307
   EXPOSED_HORIZON_PORT=8081
   EXPOSED_REDIS_PORT=7000
   ```

If you are uncertain about any of the above, the most important items to change are the DB_PASSWORD and the RCONFIG_API_TOKEN

4. Bring up the containers

```sh
docker-compose up --build
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

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->

## Usage

The default port for the web app is 8080. So go ahead and login to yourhostname.domain.com:8080 with the rConfig default creds per the documentation.

_Please refer to the [Documentation](https://www.rconfig.com/docs)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->

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

## License

This code base for docker compose is distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- https://github.com/othneildrew/Best-README-Template/blob/master/README.md -->
