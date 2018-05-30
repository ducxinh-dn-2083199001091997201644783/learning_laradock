# Introduce
    - Clone Laradock to learning docker and config PHP, Mysql, Nginx, MongoDB

## Thống kê
- Time start: 10:50 / 17:18
- Time pending: 11:48
http://docker-ghichep.readthedocs.io/en/latest/docker-compose/


[document](http://laradock.io/#B)
[speed up on mac](http://laradock.io/documentation/#upgrading-laradock)


## Environment information
* Ubuntu 14.04 UTC
* Nginx
* MySql
    * MYSQL_USER: root
    * MYSQL_PASSWORD: root@secret123
    * MYSQL_DATABASE: app
* PHP-FPM 7.0
* Redis

xin nguyen
## Content
- [Laradock](#laradock)
- [Installation](#installation)
- [Run Project](#run)
- [Docker](#docker)
- [Requirement](#requirement)
- [Redis](#redis)
- [Docker](#docker)
- [Docker vitual host](#run-a-docker-vitual-host)

<a name="a"></a>
## Requirements
|Linux|Window & Mac|
|-|-|
|Git|Git|
|Docker Engine|Docker Toolbox|
|Docker-compose||

## Laradock [document](http://laradock.io/)

### Installation
1. Setup for single Project
    * Already have a PHP project
    * Don't have a PHP project
2. Setup for Multiple project
    * Clone this repository anywhere on your machine
```
git clone https://github.com/laradock/laradock.git
```
    * Your folder structure should look like this:
```
+ laradock
+ project-1
+ project-2
```
    * Go to `nginx/sites` and create config files to point to different project directory when visiting different domains
        * Laradock by default includes app.conf.example, laravel.conf.example and symfony.conf.example as working samples
    * change the default names \*.conf:
        * You can rename the config files, project folders and domains as you like, just make sure the root in the config files, is pointing to the correct project folder name
    *  Add the domains to the hosts files.
```
127.0.0.1  project-1.test
127.0.0.1  project-2.test
.. .
```
    * Enter the laradock folder and rename env-example to .env.
```
cp env-example .env
```
    *  Run your containers:
    ```
    docker-compose up -d nginx mysql redis workspace
    docker-compose up nginx mysql redis workspace // show lỗi nếu có
    ```

### Usage
*Read Before starting:*

## Installation
```
cd laradock
docker-compose build
```
## Run
* Shell to the Workspace container
```
docker exec -it laradock_workspace_1 bash
```
* Permistion
```
chmod -R 777 storage/
```
* Install Composer
```
composer install
```
* Open /etc/hosts file and add this line:
```
xx.xx.xx.xx  server_name.com
```
* Open your browser and type server_name.com to run project

## Doker
* List current running Containers
```
docker ps -a
docker-compose ps => cụ thể, check các container k start
```
or
```
docker-compose ps //You can also use the this command if you want to see only this project containers:
```

* Remove container
```
docker rm ID_or_Name ID_or_Name
```

* Stop and remove all containers
```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

* Remove a container and its volume
```
docker rm -v container_name
``

* List Images
```
docker images -a
```
* Remove images
```
docker rmi image_name
```

* Remove all images
```
docker rmi $(docker images -a -q)
```



* List Volume
```
docker volume ls
```

* Remove Volume
```
docker volume rm volume_name
```


* Check log
```
    docker logs initial_laradock_mysql_1
```
* Close all running containers
```
docker-compose stop {container-name: optional}
```
* Build/Rebuild Containers
If you do any change to any Dockerfile make sure you run this command, for the changes to take effect:
```
docker-compose build
```
Optionally you can specify which container to rebuild (instead of rebuilding all the containers):
```
docker-compose build {container-name}
```
You might use the --no-cache option if you want full rebuilding (docker-compose build --no-cache {container-name}).

* View the Log files
The NGINX Log file is stored in the logs/nginx directory.
However to view the logs of all the other containers (MySQL, PHP-FPM,…) you can run this:
```
docker-compose logs {container-name}
docker-compose logs -f {container-name}
Options 
    -f: Follow log output
    --tail="all": Number of lines to show from the end of the logs for each container.
    -t, --timestamps    Show timestamps
```
* Install PHP Extensions
Before installing PHP extensions, you have to decide whether you need for the FPM or CLI because each lives on a different container, if you need it for both you have to edit both containers.

The PHP-FPM extensions should be installed in php-fpm/Dockerfile-XX. (replace XX with your default PHP version number). 
The PHP-CLI extensions should be installed in workspace/Dockerfile.
* Change the (PHP-FPM) Version
Open the `.env.` and Set the desired version number:
```
PHP_VERSION=5.6
```
Finally rebuild the image
```
docker-compose build php-fpm
```
* Install xDebug
open the `docker-compose.yml` and set `INSTALL_XDEBUG=true`
or `WORKSPACE_INSTALL_XDEBUG=true` in `.env` 

Open laradock/workspace/xdebug.ini and laradock/php-fpm/xdebug.ini and enable at least the following configurations:
```
xdebug.remote_autostart=1
xdebug.remote_enable=1
xdebug.remote_connect_back=1
```
Re-build the containers docker-compose build workspace php-fpm

* Change the PHP-CLI Version
By default PHP-CLI 7.0 is running.
Note: it’s not very essential to edit the PHP-CLI version. The PHP-CLI is only used for the Artisan Commands & Composer. It doesn’t serve your Application code, this is the PHP-FPM job.

The PHP-CLI is installed in the Workspace container. To change the PHP-CLI version you need to simply change the PHP_VERSION in te .env file as follow:

Open the .env and Set the desired version number:
```
PHP_VERSION=7.2
```
 Finally rebuild the image
 ```
 docker-compose build workspace
 ```


## Redis
* First make sure you run the Redis Container
```
docker-compose up -d redis
```
To execute redis commands, enter the redis container first docker-compose exec redis bash then enter the redis-cli.

* Open your `Laravel’s .env` file and set the  `REDIS_HOST` to your `docker-IP` instead of the default `127.0.0.1` IP
```
REDIS_HOST=xxx.xxx.xxx.xxx
```
* To enable Redis Caching and/or for Sessions Management. Also from the .env file set CACHE_DRIVER and SESSION_DRIVER to redis instead of the default file.
```
CACHE_DRIVER=redis
SESSION_DRIVER=redis
```
* Finally make sure you have the predis/predis package (~1.0/~) installed via Composer first
```
composer require predis/predis:^1.0
```
* You can manually test it from Laravel with this code:
```
\Cache::store('redis')->put('LaraDock', 'Awesome', 10);
```

## Use Mongo
1. First install mongo in the Workspace and the PHP-FPM Containers: 
    - open the docker-compose.yml file
    - search for the INSTALL_MONGO argument under the Workspace Container and set to `true`
    - search for the INSTALL_MONGO argument under the PHP-FPM Container and set to `true`
    - It should be:
```
args:
                - INSTALL_MONGO=true
```
2. Re-build the containers `docker-compose build workspace php-fpm`
3. Run the MongoDB Container (mongo) with the docker-compose up command.
```
docker-compose up -d mongo
```
4. Add the MongoDB configurations to the config/database.php configuration file:
```
'connections' => [

    'mongodb' => [
        'driver'   => 'mongodb',
        'host'     => env('DB_HOST', 'localhost'),
        'port'     => env('DB_PORT', 27017),
        'database' => env('DB_DATABASE', 'database'),
        'username' => '',
        'password' => '',
        'options'  => [
            'database' => '',
        ]
    ],

    // .. .

],
```
5. Open your Laravel’s .env file and update the following variables:
    - Set the `DB_HOST` to your `mongo.`
    - Set the DB_PORT to 27017.
    - set the DB_DATABASE to database

6. Finally make sure you have the jenssegers/mongodb package installed via Composer and its Service Provider is added
```
composer require jenssegers/mongodb
```
    - More detail about this [here](https://github.com/jenssegers/laravel-mongodb#installation)
7. Test it
    - First let your Models extend from the Mongo Eloquent Model. Check the [documentation](https://github.com/jenssegers/laravel-mongodb#eloquent)
    - Enter the Workspace Container
    - Migrate the Database php artisan migrate.

## Use Beanstalkd

## Access workspace via ssh

## Change the (MySQL) Version
By default MySQL 8.0 is running
MySQL 8.0 is a development release. You may prefer to use the latest stable version, or an even older release. If you wish, you can change the MySQL image that is used.

Open up your .env file and set the MYSQL_VERSION variable to the version you would like to install.
```
MYSQL_VERSION=5.7
```
Available versions are: 5.5, 5.6, 5.7, 8.0, or latest. See `https://store.docker.com/images/mysql` for more information.
## MySQL access from host
You can forward the MySQL/MariaDB port to your host by making sure these lines are added to the mysql or mariadb section of the docker-compose.yml
```
ports:
    - "3306:3306"
```
Change MySQL port
Modify the mysql/my.cnf file to set your port number, 1234 is used as an example.
```
[mysqld]
port=1234
```
If you need MySQL access from your host, do not forget to change the internal port number ("3306:3306" -> "3306:1234") in the docker-compose configuration file.

## Use custom Domain (instead of the Docker IP)
Assuming your custom domain is laravel.test
1. Open your /etc/hosts file and map your localhost address 127.0.0.1 to the laravel.test domain, by adding the following:
```
127.0.0.1    laravel.test
```
2. Open your browser and visit {http://laravel.test}
Optionally you can define the server name in the NGINX configuration file, like this:
```
server_name laravel.test;
```

## MySQL root access
The default username and password for the root MySQL user are root and root
1. Enter the MySQL container: docker-compose exec mysql bash.
2. Enter mysql: mysql -uroot -proot for non root access use mysql -uhomestead -psecret.
3. See all users: SELECT User FROM mysql.user;
4. Run any commands show databases, show tables, select * from......


## Upgrading Laradock
1. Stop the docker VM `docker-machine stop` {default}
2. Use Laradock as you used to do: `docker-compose up -d nginx mysql`.
Note: If you face any problem with the last step above: rebuild all your containers docker-compose build --no-cache “Warning Containers Data might be lost!”

## Improve speed on MacOS



## Adding cron jobs
You can add your cron jobs to workspace/crontab/root after the php artisan line.
```
* * * * * php /var/www/artisan schedule:run >> /dev/null 2>&1

# Custom cron
* * * * * root echo "Every Minute" > /var/log/cron.log 2>&1
```

## Run a Docker Vitual Host
1. Run the specical Host
```
docker-machine start MACHINE_NAME
```
* If the host doesn't exist, create a `vitualbox vitual machine` using the command below, else skip it
```
docker-machine create -d virtualbox MACHINE_NAME
```
It was store at `~/.docker/machine/machines/MACHINE_NAME/`

2. Run this command to configure your shell:
```
eval $(docker-machine env MACHINE_NAME)
```
3. Find your docker IP Address

* On Window & Mac
```
docker-machine ip MACHINE_NAME
```
<small>(The default IP is 192.168.99.100)</small>

* On Linux
```
ifconfig docker0 | grep 'inet' | cut -d: -f2 | awk '{ print $1}' | head -n1
```
<small>(The default IP is 172.17.0.1)</small>

4. List vitual-machine
```
docker-machine ls
```

## Common Problems
Here’s a list of the common problems you might face, and the possible solutions.

1. I see a blank (white) page instead of the Laravel ‘Welcome’ page!
Run the following command from the Laravel root directory:
```
sudo chmod -R 777 storage bootstrap/cache
```

2. I see “Welcome to nginx” instead of the Laravel App
```
Use http://127.0.0.1 instead of http://localhost in your browser.
```

3. I see an error message containing address already in use or port is already allocated
    - Make sure the ports for the services that you are trying to run (22, 80, 443, 3306, etc.) are not being used already by other programs on the host, such as a built in apache/httpd service or other development tools you have installed.
4. The time in my services does not match the current time
    - Make sure you’ve changed the timezone.
    ```
    workspace:
        build:
            context: ./workspace
            args:
                - TZ=America/New_York
    .. .
    ```
    - Stop and rebuild the containers (docker-compose up -d --build <services>)
5. I get MySQL connection refused
    - Check your running Laravel application IP by dumping Request::ip()
    - Change the DB_HOST variable on env with the IP that you received from previous step.
    - or
    - Change the DB_HOST value to the same name as the MySQL docker container. The Laradock docker-compose file currently has this as mysql

6. Can't start Mongo, Mysql, Redis
    - add columns
```
volumes: 
  mongo:
    driver: ${VOLUMES_DRIVER}
  mysql:
    driver: ${VOLUMES_DRIVER}
```
```
mongo:
    volumes:
        - ${DATA_PATH_HOST}/mongo:/data/db

to

mongo:
    volumes:
        - ${DATA_PATH_HOST}/mongo:/data

```

8. libpng12-dev
```
    replace to libpng-dev
```

9. Docker build timeout
```
RUN curl -fsSL https://test.docker.com | sh
RUN apt-get clean
```

8. The server requested authentication method unknown to the client [caching_sha2_password
    - Change version Mysql to 5.7 `FROM mysql:5.7`
    - rm -rf ~/.laradock/data/mysql
    - docker-compose build mysql
    - docker-compose up -d mysql

6. SSH connection failed! - The SSH Tunnel has unexpectedly closed."
    -> Nhấn vào detail
    - vim /Users/xinhnguyen/.ssh/knowhost  remove dòng chứa ip chẳng hạn `docker-machine ip` của docker
    - 
7. Can't start Mysql
- `docker-compose up mysql`

```
[Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.

[ERROR] [MY-011071] [Server] /usr/sbin/mysqld: Error while setting value 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION' to 'sql_mode'

[ERROR] [MY-010119] [Server] Aborting
```
- resole
```
docker-compose.yml: 
    mysql:
        driver: ${VOLUMES_DRIVER}

.env file
    MYSQL_VERSION=latest to MYSQL_VERSION=5.7
```

    - add columns
```
volumes: 
  mongo:
    driver: ${VOLUMES_DRIVER}
  mysql:
    driver: ${VOLUMES_DRIVER}
```
```
mongo:
    volumes:
        - ${DATA_PATH_HOST}/mongo:/data/db

to

mongo:
    volumes:
        - ${DATA_PATH_HOST}/mongo:/data

```

8. Named volume "db_data:/var/lib/mysql:rw" is used in service "mysql" but no declaration was found in the volumes section.

```
services:
    mysql:
        build:  ...
        environment: ... 
        volumes:
            - db_data:/var/lib/mysql

```

    - Resole: thêm đường dẫn tương đối. `../xxx` instead `xxx
```
    services:
    mysql:
        build:  ...
        environment: ... 
        volumes:
            - "../db_data:/var/lib/mysql"
```
    - Hoặc define volume name: 
```
    volumes
        db_data:
            driver: local  #is already local by default
    services:
    mysql:
        build:  ...
        environment: ... 
        volumes:
            - db_data:/var/lib/mysql

```
 9.  Can't start mysql

    - Resole: 
```
version: '3'
volumes:
    mysql: # Ten volumn
        driver: ${VOLUMES_DRIVER} # default local
services: 

### MySQL ################################################
    mysql:
      build:
        context: ./mysql
        args:
          - MYSQL_VERSION=${MYSQL_VERSION}
      environment:
        - MYSQL_DATABASE=${MYSQL_DATABASE}
        - MYSQL_USER=${MYSQL_USER}
        - MYSQL_PASSWORD=${MYSQL_PASSWORD}
        - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
        - TZ=${WORKSPACE_TIMEZONE}
      restart: always
    
      volumes:
        - ${DATA_PATH_HOST}/mysql:/var/lib/mysql
        - ${MYSQL_ENTRYPOINT_INITDB}:/docker-entrypoint-initdb.d

    replace volumes  to

      volumes:
          - mysql:/var/lib/mysql # mysql tên local volume đã khai báo ở volumns
      ports:
        - "${MYSQL_PORT}:3306"
      networks:
        - backend

 ```
