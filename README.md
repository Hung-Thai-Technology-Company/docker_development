Docker Compose for PHP Projects
==============
This is a complete stack for running Laravel into Docker containers using docker-compose tool.

# Installation

Next, add `user.czytest.savour.local` in your `/etc/hosts` file.

Make sure you adjust `databases info` in `env` to the database container alias "db" for mysql and "redis" for redis connection

```bash
APP_URL=http://user.czytest.savour.local

DB_CONNECTION=mysql
DB_READ_HOST=db
DB_READ_PORT=3306
DB_READ_DATABASE=micro_business_district
DB_READ_USERNAME=root
DB_READ_PASSWORD=root

DB_WRITE_HOST=db
DB_WRITE_PORT=3306
DB_WRITE_DATABASE=micro_business_district
DB_WRITE_USERNAME=root
DB_WRITE_PASSWORD=root

REDIS_HOST=redis
REDIS_PORT=6379
```
Then, run:

```bash
$ docker-compose up -d
```

You are done, you can visit your Laravel application on the following URL: `http://wsq.savour.local`

_Note :_ you can rebuild all Docker images by running:

```bash
$ docker-compose build
```

# How it works?

Here are the `docker-compose` built images:

* `db`, `redis`: This is the MySQL & Redis databases container (can be changed to postgresql or whatever in `docker-compose.yml` file),
* `php`: This is the PHP-FPM container including the application volume mounted on,
* `nginx`: This is the Nginx webserver container in which php volumes are mounted too

This results in the following running containers:

```bash
> $ docker-compose ps
 Name                Command              State              Ports
------------------------------------------------------------------------------
db        docker-entrypoint.sh --def      Up      0.0.0.0:3308->3306/tcp, 33060/tcp
nginx     nginx                           Up      443/tcp, 0.0.0.0:80->80/tcp
php-fpm   php-fpm7 -F                     Up      0.0.0.0:9000->9001/tcp
redis     redis-server --appendonly yes   Up      0.0.0.0:6379->6379/tcp
```

# Backend API Platform:

## Install: 
```bash
> $ cd www/pmh_czy_wsq_service
# rename file .env.dist to .env

> $ docker exec -it php-fpm sh
> $ cd /var/www/pmh_czy_wsq_service

# Install/update composer dependecies
> $ composer install

# Run database migrations
> $ php artisan migrate --force

# Clear caches
> $ php artisan cache:clear
```

## API URI: 
`http://user.czytest.savour.local/api`

## Swagger API Docs:
`http://user.czytest.savour.local.local/explorer/index.html`